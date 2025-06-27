---
layout: default
title: 统计
---
<div class="well article">
    <a id="{{ category-analysis }}" style="position: relative; top: -50px"></a>
    <h2>分类</h2>
    <!-- Find the max category count -->
    {% assign sorted_categories = site.categories | sort %}
    {% assign max_category_count = 0 %}
    {% for category in sorted_categories %}
        {% if category[1].size > max_category_count %}
            {% assign max_category_count = category[1].size %}
        {% endif %}
    {% endfor %}
    <!-- Begin display -->
    {% for i in (1..max_category_count) reversed %}
        {% for category in sorted_categories %}
            {% if category[1].size == i %}
                <a href="{{ site.baseurl }}/categories#{{ category[0] }}">{{ category[0] }} ({{ category[1].size }})</a>
                &nbsp;|&nbsp;
            {% endif %}
        {% endfor %}
    {% endfor %}
    <span><b>分类个数：{{ site.categories | size }}</b></span>
</div>

<div class="well article">
    <a id="{{ tag-analysis }}" style="position: relative; top: -50px"></a>
    <h2>标签</h2>
    <!-- Find the max tag count -->
    {% assign sorted_tags = site.tags | sort %}
    {% assign max_tag_count = 0 %}
    {% for tag in sorted_tags %}
        {% if tag[1].size > max_tag_count %}
            {% assign max_tag_count = tag[1].size %}
        {% endif %}
    {% endfor %}
    <!-- Begin display -->
    {% for i in (1..max_tag_count) reversed %}
        {% for tag in sorted_tags %}
            {% if tag[1].size == i %}
                <a href="{{ site.baseurl }}/tags#{{ tag[0] }}">{{ tag[0] }} ({{ tag[1].size }})</a>
                &nbsp;|&nbsp;
            {% endif %}
        {% endfor %}
    {% endfor %}
    <span><b>标签个数：{{ site.tags | size }}</b></span>
</div>

<div class="well article">
    <a id="{{ character-analysis }}" style="position: relative; top: -50px"></a>
    <h2>人物</h2>
    <!-- Look for the name list of all characters -->
    {% assign character_list = "" | split: "" %}
    {% for node in site.posts %}
        {% for character in node.characters %}
            {% unless character_list contains character %}
                {% assign character_list = character_list | push: character %}
            {% endunless %}
        {% endfor %}
    {% endfor %}
    {% assign character_list = character_list | sort %}
    {% assign character_num = character_list | size %}
    <!-- Look for the count for each character -->
    {% assign character_count_list = "" | split: "" %}
    {% for character in character_list %}
        {% assign character_count = 0 %}
        {% for node in site.posts %}
            {% for node_character in node.characters %}
                {% if node_character == character %}
                    {% assign character_count = character_count | plus: 1 %}
                    {% break %}
                {% endif %}
            {% endfor %}
        {% endfor %}
        {% assign character_count_list = character_count_list | push: character_count %}
        {% assign character_count = 0 %}
    {% endfor %}
    <!-- Look for the max character count -->
    {% assign sorted_character_count = character_count_list | sort | reverse %}
    {% assign max_character_count = sorted_character_count[0] %}
    <!-- Begin display -->
    {% assign character_num_minus = character_num | minus: 1 %}
    {% for character_count_index in (1..max_character_count) reversed %}
        {% for i in (0..character_num_minus) %}
            {% if character_count_list[i] == character_count_index %}
                <a href="{{ site.baseurl }}/characters#{{ character_list[i] }}">{{ character_list[i] }} ({{ character_count_list[i] }})</a>
                &nbsp;|&nbsp;
            {% endif %}
        {% endfor %}
    {% endfor %}
    <span><b>人物个数：{{ character_num }}</b></span>
</div>

<div class="well article">
    <a id="{{ date-analysis }}" style="position: relative; top: -50px"></a>
    <h2>日期</h2>
    {%for post in site.posts %}
        <!-- Display year -->
        {% if post.next == nil %}
            <h3>{{ post.date | date: '%Y' }}</h3>
            {% assign year_count = 0 %}
        {% else %}
            {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
            {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
            {% if year != nyear %}
                <h3>{{ post.date | date: '%Y' }}</h3>
                {% assign year_count = 0 %}
            {% endif %}
        {% endif %}
        <!-- Analysis month -->
        {% if post.previous != nil %}
            {% capture month %}{{ post.date | date: '%B' }}{% endcapture %}
            {% if post.next == nil %}
                {% assign month_count = 0 %}
            {% else %}
                {% capture nmonth %}{{ post.next.date | date: '%B' }}{% endcapture %}
                {% if month != nmonth %}
                    {% assign month_count = 0 %}
                {% endif %}
            {% endif %}
            {% assign month_count = month_count | plus: 1 %}
            {% capture pmonth %}{{ post.previous.date | date: '%B' }}{% endcapture %}
            {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
            {% capture pyear %}{{ post.previous.date | date: '%Y' }}{% endcapture %}
            {% if month != pmonth or year != pyear %}
                {{ month }} ({{ month_count }})
                &nbsp;|&nbsp;
                {% assign year_count = year_count | plus: month_count %}
                {% assign month_count = 0 %}
                <!-- Display year count -->
                {% if year != pyear %}
                    <b>总数：{{ year_count }}</b>
                {% endif %}
            {% endif %}
        {% else %}
            {% assign month_count = month_count | plus: 1 %}
            {{ pmonth }} ({{ month_count }})
            {% assign year_count = year_count | plus: month_count %}
            {% assign month_count = 0 %}
            <!-- Display year count -->
            &nbsp;|&nbsp;
            <b>总数：{{ year_count }}</b>
            {% assign year_count = 0 %}
        {% endif %}
    {% endfor %}
</div>

<div class="well article">
    <a id="{{ total-analysis }}" style="position: relative; top: -50px"></a>
    <h2>总数</h2>
    <ul>
        <li>总文章数：{{ site.posts | size }}</li>
        {% assign total_word_count = 0 %}
        {% for post in site.posts %}
            {% assign post_word_count = post.content | strip_html | strip_newlines | number_of_words %}
            {% assign total_word_count = total_word_count | plus: post_word_count %}
        {% endfor %}
        <li>总字数：{{ total_word_count }}</li>
        <script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
        <li>
            <span id="busuanzi_container_site_pv">
                总访问量：<span id="busuanzi_value_site_pv"></span>
            </span>
        </li>
        <li>
            <span id="busuanzi_container_site_uv">
                总访客数：<span id="busuanzi_value_site_uv"></span>
            </span>
        </li>
    </ul>
</div>