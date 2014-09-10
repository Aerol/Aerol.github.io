---
layout: post
title:  "First post"
date:   2014-09-09 11:30:40
tags: jekyll post
---
First test blog post, will be updated when everything is working all legit like

Some test code highlighting

{% highlight c linenos %}
#include <linux/init.h>
#include <linux/module.h>
#include <linux/kernel.h>

/* main function, called on module load */

static int hello_init(void)
{
    printk("Hello World!\n");
    return 0;
}

static void hello_exit(void)
{
    printk("Goodbye World!\n");
}

module_init(hello_init);
module_exit(hello_exit);

MODULE_LICENSE("GPL");
MODULE_AUTHOR("Henry Case");
MODULE_DESCRIPTION("Eudyptula Challenge 1, Hello World module");
{% endhighlight %}

    obj-m += task01.o

    all:
    ifeq ($(KERN_SRC), )
        make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules
    else
        make -C $(KERN_SRC) M=$(PWD) modules
    endif


Build kernel module with:

    make

or

    make /path/to/kernel/source/
