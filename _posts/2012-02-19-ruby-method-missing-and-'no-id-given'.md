---
title: Ruby, method_missing and 'no id given'
enki_id: 26
tags: [ruby, rails, metaprogramming]
---
**Note**: if you just happen to be interested in knowing possible causes for the 'no id given' error message in [Ruby](http://en.wikipedia.org/wiki/Ruby_(programming_language)), go to the last paragraph of this post.

I don't have reason to do a lot of Ruby metaprogramming myself, although being a Rails user, I surely receive a lot of benefit from it. Metaprgramming is used extensively in Rails, the most visible example I can think of is the [ActiveRecord Dynamic Finders](http://guides.rubyonrails.org/active_record_querying.html#dynamic-finders).<!--more-->

Working on my current pet project, I had occasion to do a little metaprogramming in a similar vein to the ActiveRecord Dynamic Finders. I have a model with two sets of [paperclip](https://github.com/thoughtbot/paperclip/) attachments. One is for when an attachment is first uploaded to my server and the other is for when the attachment is subsequently moved to [Amazon S3](http://aws.amazon.com/s3/) for permanent storage. The file stored on my server is deleted after successful transfer to Amazon S3.

I wanted some convenience accessor methods that would return values from whichever attachment actually existed on the model. The key to this is to implement the `method_missing` method, which is called when Ruby receives a message invoking a method that doesn't exist in the current context. The use of `method_missing` is a common approach in Ruby metaprogramming. My code initially looked something like:

```ruby
class Asset < ActiveRecord::Base

  ...

  def method_missing(method_id, *arguments, &block)
    method_id = method_id.to_s

    if method_id =~ /^upload_(file_name|file_size|content_type|updated_at)$/
      self.class.send :define_method, method_id do
        if self.local_upload?
          eval("self.local_upload_#{$1}")
        elsif self.s3_upload?
          eval("self.s3_upload_#{$1}")
        elsif self.file_deleted?
          if method_id == "upload_file_size"
            0
          else
            "File deleted"
          end
        else
          "Available soon..."
        end
      end
      self.send(method_id)
    else
      super
    end
  end

  def respond_to?(method_id, include_private = false)
    if method_id.to_s =~ /^upload_(file_name|file_size|content_type|updated_at)$/
      true
    else
      super
    end

  ...

  end

  # The idea being that I could then for example, write:
  a = Asset.find(1)

  # Extra-dynamic finder that will return the value of a.local_upload_file_name if present,
  # return the value of a.s3_upload_file_name if the local upload doesn't exist,
  # and finally some overly hacky business to handle return values in the case of neither
  # file being present or the file not yet having finished processing in the web server.
  a.upload_file_name
```

This was my first attempt and I had a number of concerns with this code. But I'm going to concentrate on one issue in particular that got me pretty good. Note line 6 where I convert the @method_id@ variable to a string (originally it's a symbol). I absentmindedly did this just so that I could convert it once and use it as a string in two different places (lines 8 and 15). It turns out one doesn't really need to convert a symbol to a string in order to use it with the pattern matching operator, but I initially thought I'd make things nice and explicit so as to avoid possible issues. In hindsight this now seems a little ironic.

So moving on, I refresh my browser only to find this rather terse error message:

```
no id given
```

Was this some kind of Rails routing error? No, looking at the stack trace, it stopped at:

```
activemodel (3.0.9) lib/active_model/attribute_methods.rb:392:in `method_missing'
```

So this prompted me to think that my metaprogramming attempt had stuffed something up quite nicely. My `Asset#method_missing` method was passing the call to `super` and [line 392 of attribute_methods.rb](https://github.com/rails/rails/blob/v3.0.9/activemodel/lib/active_model/attribute_methods.rb#L392) was another call to `super`. I couldn't find where this error message was actually coming from. A little more investigation seemed to reveal that this error actually comes from Ruby, not Rails. From [line 496 of vm_eval.c in Ruby 1.9.3-p0](https://github.com/ruby/ruby/blob/v1_9_3_0/vm_eval.c#L496) to be exact:

```c
raise_method_missing(rb_thread_t *th, int argc, const VALUE *argv, VALUE obj,
		     int last_call_status)
{
    ID id;
    VALUE exc = rb_eNoMethodError;
    const char *format = 0;

    if (argc == 0 || !SYMBOL_P(argv[0])) {
	rb_raise(rb_eArgError, "no id given");
    }

  ...

}
```

Assuming the call to `method_missing` makes it all the way to the `BasicObject` class, Ruby 1.9.3-p0 doesn't like it when you pass in a string instead of a symbol to `BasicObject#method_missing` and you get 'no id given'. It would also appear that this same error results if you try to invoke `BasicObject#method_missing` with no arguments. The 'no id given' error message making a little more sense in this latter case than the former.
