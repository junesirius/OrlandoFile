---
layout: default
title: 图片
---
{% assign tags = site.tags %}

<div class="well article">
{% for tag in tags %}
    {% if tag[0] == "画" %}
        <a id="画" style="position: relative; top: -50px"></a>
        <h2>画</h2>
        {% assign pages_list = tag[1] %}
        {% for page in pages_list %}
            {% assign images = page.content | split: "<img src=" %}
            {% for image in images %}
                {% assign img = image | split: ' alt="" />' | first %}
                {% if img contains site.baseurl %}
                    <a href="{{ site.baseurl }}{{ page.url }}"><img src={{ img }} style="height: 200px; width: auto; object-fit: scale-down; margin-bottom: 5px;"></a>
                {% endif %}
            {% endfor %}
        {% endfor %}
        {% assign pages_list = nil %}
</div>
    {% elsif tag[0] == "摄影" %}
<div class="well article">
        <a id="摄影" style="position: relative; top: -50px"></a>
        <h2>摄影</h2>
        {% assign pages_list = tag[1] %}
        {% for page in pages_list %}
            {% assign images = page.content | split: "<img src=" %}
            {% for image in images %}
                {% assign img = image | split: ' alt="" />' | first %}
                {% if img contains site.baseurl %}
                    <a href="{{ site.baseurl }}{{ page.url }}"><img src={{ img }} style="height: 200px; width: auto; object-fit: scale-down; margin-bottom: 5px;"></a>
                {% endif %}
            {% endfor %}
        {% endfor %}
        {% assign pages_list = nil %}        
    {% endif %}
{% endfor %}
</div>
