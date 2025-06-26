---
layout: default
title: 统计
---
<div class="well article">
    <a id="{{ longnovel-analysis }}" style="position: relative; top: -50px"></a>
    <h2>长篇</h2>
    <!-- Look for the name list of all long-novels -->
    {% assign long_novel_posts = site.posts | where_exp:"post", "post.long_novels != null" %}
    {% assign long_novel_list = "" | split: "" %}
    {% for post in long_novel_posts %}
        {% unless long_novel_list contains post.long_novels %}
            {% assign long_novel_list = long_novel_list | push: post.long_novels %}
        {% endunless %}
    {% endfor %}
    {% assign long_novel_list = long_novel_list | sort %}
    <!-- Look for count of each longnovel -->
    {% assign long_novel_count_list = "" | split: "" %}
    {% for long_novel in long_novel_list %}
        {% assign long_novel_count = 0 %}
        {% for post in long_novel_posts %}
            {% if post.long_novels == long_novel %}
                {% assign long_novel_count = long_novel_count | plus: 1 %}
            {% endif %}
        {% endfor %}
        {% assign long_novel_count_list = long_novel_count_list | push: long_novel_count %}
        {% assign long_novel_count = 0 %}
    {% endfor %}
    <!-- Look for max longnovel count -->
    {% assign long_novel_num = long_novel_list | size %}
    {% assign long_novel_num_minus = long_novel_num | minus: 1 %}
    {% assign sorted_long_novel_count = long_novel_count_list | sort | reverse %}
    {% assign max_long_novel_count = sorted_long_novel_count[0] %}
    <!-- Begin display -->
    <ul>
    {% for long_novel_index in (1..max_long_novel_count) reversed %}
        {% for i in (0..long_novel_num_minus) %}
            {% if long_novel_count_list[i] == long_novel_index %}
                {% assign long_novel_wordcount = 0 %}
                {% assign long_novel_finished = false %}
                {% assign long_novel_update_time = "" %}
                {% for post in long_novel_posts %}
                    {% if post.long_novels == long_novel_list[i] %}
                        {% assign post_wordcount = post.content | strip_html | strip_newlines | size %}
                        {% assign long_novel_wordcount = long_novel_wordcount | plus: post_wordcount %}
                        {% if long_novel_update_time == "" %}
                            {% assign long_novel_update_time = post.date %}
                        {% endif %}
                        {% if post.tags contains "已完结" %}
                            {% assign long_novel_finished = true %}
                        {% endif %}
                    {% endif %}
                {% endfor %}
                <li>
                    <div class="row" style="margin: 0; padding: 0;">
                    <div class="col-md-7" style="margin: 0; padding: 0">
                        {% if long_novel_finished == true %}
                            <a href="{{ site.baseurl }}/longnovels#{{ long_novel_list[i] }}">{{ long_novel_list[i] }}（已完结）</a>
                        {% else %}
                            <a href="{{ site.baseurl }}/longnovels#{{ long_novel_list[i] }}">{{ long_novel_list[i] }}</a>
                        {% endif %}
                    </div>
                    <div class="col-md-3" style="margin: 0; padding: 0">
                        {{ long_novel_count_list[i] }}篇，{{ long_novel_wordcount }}字
                    </div>
                    <div class="col-md-2" style="margin: 0; padding: 0">
                        <span class="post-date">
                            {{ long_novel_update_time | date: site.date_format.tags }}
                        </span>
                    </div>
                    </div>
                </li>
                {% assign long_novel_wordcount = 0 %}
                {% assign long_novel_finished = false %}
                {% assign long_novel_update_time = "" %}
            {% endif %}
        {% endfor %}
    {% endfor %}
    </ul>
</div>