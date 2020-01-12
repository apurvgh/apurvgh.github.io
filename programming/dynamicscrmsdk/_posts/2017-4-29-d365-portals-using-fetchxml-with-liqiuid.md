---
layout: post-default
title: D365 Portals - Using FetchXML with Liqiuid
date: 2017-4-29 12:5:8 - 0600
PostDate: 2017-4-29
author: apurvghai
ag_categories: Dynamics CRM SDK
lang: C#

---
Hello Everyone

This is my first ever post with D365 Portals. I will bring up more interesting ones as we go along. Today I am going to talk about how you can customize your portal's web template to use CRM fetch query with <a href="https://www.microsoft.com/en-us/dynamics/crm-setup-and-administration/liquid-objects.aspx">liquid</a>.

<strong>Prerequisites:</strong>
<ul>
 	<li>FetchXML using Advanced Find in CRM</li>
 	<li>Customizing D365 Portals</li>
</ul>
<strong>Scenario:</strong>

D365 CRM provides an ability to categorize your articles using ISH (Interactive Service Hub). Those categories are used as tags and then displayed on D365 Portals as a list to browse the articles from. I will showcase how to modify the existing template and using FetchXML instead.

<strong>Build your FetchXML using Advanced Find</strong>
<ul>
 	<li>Go to Advanced Find</li>
 	<li>Select the entity as "Categories"</li>
 	<li>See FetchXML Sample</li>
</ul>
	{% raw %}
		<fetch mapping="logical" returntotalrecordcount="true" count="{{ count }}">
		<entity name="category">
		<attribute name="categorynumber" />
		<attribute name="title" />
		<attribute name="createdon" />
		<attribute name="categoryid" />
		<order descending="false" attribute="title" />
		</entity>
		</fetch>
	{% endraw %}

**Review existing web template:**

- Go to D365 Online Instance - CRM
- Go to Portals > Web Templates > Knowledge Base Home
- Open the record
- You will see the below code:

	{% raw %}
	
		{% extends 'layout_1_column' %}
		{% block main %}
		{% include 'Page Copy' %}
		{% assign category_url = sitemarkers['Category'].url %}
		{% assign count = count | default: 0 %}
		{% assign categories = knowledge.categories | top_level: count %}
		{% if categories %}
		<div class="list-group unstyled">
		{% for category in categories %}
		<a href="'id', category.categorynumber }}" class="list-group-item">
		{{ category.title }}
		</a>
		{% endfor %}
		</div>
		{% endif %}
	
	{% endraw %}

**Modify the Web Template:**
 - Add the liqiuid objects in following format

<!-- New Category Display -->

{% raw %}
		<!-- OLD Category Display -->;
		{% extends 'layout_1_column' %}
		{% block main %}
		{% include 'Page Copy' %}
		{% assign category_url = sitemarkers['Category'].url %}
		{% assign count = count | default: 0 %}
		{% assign categories = knowledge.categories | top_level: count %}
		{% if categories %}
		<div class="list-group unstyled">
		{% for category in categories %}
		<!-- <a href="'id', category.categorynumber }}" class="list-group-item">-->
		{{ category.title }}
		</a> -->;
		{% endfor %}
		</div>

		{% endif %}
		<strong><!-- OLD Category Display -->;</strong>

		<!-- New Category Display -->;
		<div class="list-group">
		{% fetchxml categories_query %}

		{% endfetchxml %}
		{% assign categories = categories_query.results.entities %}
		{% if categories %}
		<div class="list-group">
		<h4 class="list-group-item-heading">Articles by Categories</h4> <br /> <br />
		{% for category in categories %}
		<a class="list-group-item" href="'id', category.categorynumber }}{{ category.categorynumber | escape }}">{{ category.title | escape }}</a>
		{% endfor %}
		</div>
		{% endif %}
		<strong><!-- End New Category Display -->;</strong>
		</div>;
		{% endblock %}

{% endraw %}


You will see the categories are now sorted in order.

<a href="http://apurvghai.files.wordpress.com/2017/04/categories.png"><img src="https://msdnshared.blob.core.windows.net/media/2017/04/Categories-300x76.png" alt="categories" width="300" height="76" class="alignleft size-medium wp-image-715" /></a>

Hope you had fun customizing your Portals :)
<br/>
-Apurv
