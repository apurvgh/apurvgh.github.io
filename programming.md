---
title: Programming
layout: post-default
---

Hey folks! Welcome to my programming section. I have figured my thing is APIs so I have put together some interesting reads on my favorite technologies using different programming languages.

Read and enjoy!
<br/><br/>
#### Categories
---

 <div>
        <div class="container">
            <div class="row seperator-top-bottom">
                <div class="col-lg-12 seperator-top-bottom">
                    <div class="row">
                        <div class="col-lg-6">
                            <h5 class="mb-3"><i class="far fa-bookmark"></i>&nbsp;&nbsp;Dynamics 365</h5>
                            <div class="col-xs-12">
                                {% for post in site.posts %}
                                {% for category_name in post.categories %}
                                {% if category_name == "dynamicscrmsdk" %}
                                <div class="post-title">
                                    <a href="{{ post.url }}">{{ post.title }} </a><br>
                                    {{ post.PostDate | date: "%b %d, %y" }} | <i class="ag_categories">{{ post.ag_categories}} </i>
                                </div>
                                {% endif %}
                                {% endfor %}
                                {% endfor %}
                            </div>
                        </div>
                        <div class="col-lg-6">
                            <h5 class="mb-3"><i class="far fa-bookmark"></i>&nbsp;&nbsp;Azure DevOps</h5>
                            <div class="col-xs-12">
                                {% for post in site.posts %}
                                {% for category_name in post.categories %}
                                {% if category_name == "azuredevops" %}
                                <div class="post-title">
                                    <a href="{{ post.url }}">{{ post.title }} </a><br>
                                    {{ post.PostDate | date: "%b %d, %y" }} | <i class="ag_categories">{{ post.ag_categories}} </i>
                                </div>
                                {% endif %}
                                {% endfor %}
                                {% endfor %}
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="row seperator-top-bottom">
                <div class="col-lg-12 seperator-top-bottom">
                    <div class="row">
                        <div class="col-lg-6">
                            <h5 class="mb-3"><i class="far fa-bookmark"></i>&nbsp;&nbsp;Exchange</h5>
                            <div class="col-xs-12">
                                {% for post in site.posts %}
                                {% for category_name in post.categories %}
                                {% if category_name == "exchange" %}
                                <div class="post-title">
                                    <a href="{{ post.url }}">{{ post.title }} </a><br>
                                    {{ post.PostDate | date: "%b %d, %y" }} | <i class="ag_categories">{{ post.ag_categories}} </i>
                                </div>
                                {% endif %}
                                {% endfor %}
                                {% endfor %}
                            </div>
                        </div>
                        <div class="col-lg-6">
                            <h5 class="mb-3"><i class="far fa-bookmark"></i>&nbsp;&nbsp;Graph / Office 365</h5>
                            <div class="col-xs-12">
                                {% for post in site.posts %}
                                {% for category_name in post.categories %}
                                {% if category_name == "graphapi" %}
                                <div class="post-title">
                                    <a href="{{ post.url }}">{{ post.title }} </a><br>
                                    {{ post.PostDate | date: "%b %d, %y" }} | <i class="ag_categories">{{ post.ag_categories}} </i>
                                </div>
                                {% endif %}
                                {% endfor %}
                                {% endfor %}
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>