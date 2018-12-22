---
layout: post
title:  "One way to execute a role several times with different variable in Ansible"
date:   2018-12-22
excerpt: "Role makes easy to maintain ansible script. You want to execute same role with different variable several times. How can we write such code?"
tag:
- Ansible 
comments: true
--- 

## Situation
Let's say you have one role and you want to make a loop with this role. 
However, the role has some variable and you need to change the value for each time.
How we can handle this without modifying original role yml?
There's one way to handle this without modifying original role yml.

## How to handle
### Original role file
{% raw %}
```
---
- name: File copy
  copy: >
    src={{ source }}
    dest={{ target }}
    mode=0755
```
{% endraw %}

### playbook we can try
{% highlight yaml %}
---
- vars_files:
    - filecopy_env
  hosts: '{{ server }}'
  become: yes
  become_user: '{{ username }}'
  tasks:
    - name: copy multiple script
      with_items: "{{ file_array }}"
      include_role:
        name: file_copy
      vars:
        source: "{{ item.source }}"
        target: "{{ item.target }}"
{% endhighlight %}

### How to write an array for this ?
Here's contents of filecopy_env file
{% highlight yaml %}
username: hogehoge
file_array:
  - { source : /home/hogehoge/hoge01.txt, target : /home/asagohan/hoge01.txt }
  - { source : /home/hogehoge/hoge02.txt, target : /home/asagohan/hoge02.txt }

{% endhighlight %}
