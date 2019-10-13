---
layout: page
title: Kothing Template for Jekyll
permalink: /about
comments: false
---

<div class="row justify-content-between">
<div class="col-md-8 pr-5">

<p>This website is built with Jekyll and Kothing template for Jekyll. Kothing template for Jekyll is compatible with Github pages, in fact even this demo is created with Github Pages and hosted with Github.</p>

<p class="mb-5"><img class="shadow-lg" src="{{site.baseurl}}/assets/images/kothing-template.jpg" alt="jekyll template kothing" /></p>
<h4>Documentation</h4>

<p>"Kothing" is a free Jekyll theme for blogging, collections, resources, reviews websites</p>

<h4>Features</h4>
<ul>
    <li>Built for Jekyll</li>
    <li>Compatible with Github pages</li>
    <li>Featured Posts</li>
    <li>Index Pagination</li>
    <li>Multi Language</li>
    <li>SEO</li>
    <li>Feed</li>
    <li>Sitemap</li>
    <li>Post Share</li>
    <li>Post Categories</li>
    <li>Prev/Next Link</li>
    <li>Category Archives (Compatible with Github pages)</li>
    <li>Jumbotron Categories</li>
    <li>Post Reviews with Stars</li>
    <li>Blurred Spoilers</li>
    <li>Table of Content</li>
    <li>Lazy Load Images</li>
    <li>Integrations:
        <ul>
            <li>Disqus Comments</li>
            <li>Google Analaytics</li>
            <li>Mailchimp Integration</li>
        </ul>
    </li>
    <li>Design Features:
        <ul>
            <li>Bootstrap v4.x</li>
            <li>Font Awesome</li>
            <li>Masonry</li>
        </ul>
    </li>
    <li>Layouts:
        <ul>
            <li>Default</li>
            <li>Post</li>
            <li>Page</li>
            <li>Archive</li>
            <li>Categories (for 100% compatibility with Github pages)</li>
        </ul>
    </li>
</ul>

<h4>What’s Jekyll</h4>

<p>If you aren’t familiar with Jekyll yet, you should know that it is a static site generator. It will transform your plain text into static websites and blogs. No more databases, slow loading websites, risk of being hacked…just your content. And not only that, with Jekyll you get free hosting with GitHub Pages! This page itself is free hosted on Github with the help of Jekyll and Kothing template that you’re currently previewing. If you are a beginner we recommend you start with <a href="https://jekyllrb.com/docs/installation/">Jekyll’s Docs</a>. Now if you know how to use Jekyll, let’s move on to using Kothing template in Jekyll:</p>

<h4>How to use "Kothing" theme</h4>

<ol>
    <li><a href="https://github.com/kothing/kothing-theme/archive/master.zip">Download</a> or <code class="highlighter-rouge">git clone https://github.com/kothing/kothing-theme.git</code></li>
    <li><code class="highlighter-rouge">cd kothing-theme</code></li>
    <li><code class="highlighter-rouge">bundle</code></li>
    <li>Edit <code class="highlighter-rouge">_config.yml</code> options. If your site is in root: <code class="highlighter-rouge">baseurl: ''</code>. Also, change your Google Analytics code, disqus username, authors, Mailchimp list etc.</li>
    <li><code class="highlighter-rouge">jekyll serve --watch</code></li>
    <li>Start by adding your <code class="highlighter-rouge">.md</code> files in <code class="highlighter-rouge">_posts</code>. Kothing already has a few examples.</li>
    <li>YAML front matter
        <ul>
            <li>featured post - <code class="highlighter-rouge">featured:true</code></li>
            <li>exclude featured post from “All stories” loop to avoid duplicated posts - <code class="highlighter-rouge">hidden:true</code></li>
            <li>post image - <code class="highlighter-rouge">image: assets/images/mypic.jpg</code></li>
            <li>external post image - <code class="highlighter-rouge">image: "https://externalwebsite.com/image4.jpg"</code></li>
            <li>page comments - <code class="highlighter-rouge">comments:true</code></li>
            <li>meta description (optional) - <code class="highlighter-rouge">description: "this is my meta description"</code></li>
        </ul>
    </li>
</ol>

<h4>YAML Post Example:</h4>
<div class="language-markdown highlighter-rouge">
    <div class="highlight">
        <table class="rouge-table">
            <tbody>
                <tr>
                    <td class="rouge-gutter gl"></td>
                    <td class="rouge-code">
<pre>
<code><span class="nn">---</span>
<span class="na">layout</span><span class="pi">:</span> <span class="s">post</span>
<span class="na">title</span><span class="pi">:</span>  <span class="s2">"</span><span class="s">We</span><span class="nv"> </span><span class="s">all</span><span class="nv"> </span><span class="s">wait</span><span class="nv"> </span><span class="s">for</span><span class="nv"> </span><span class="s">summer"</span>
<span class="na">author</span><span class="pi">:</span> <span class="s">john</span>
<span class="na">categories</span><span class="pi">:</span> <span class="pi">[</span> <span class="nv">Jekyll</span><span class="pi">,</span> <span class="nv">tutorial</span> <span class="pi">]</span>
<span class="na">image</span><span class="pi">:</span> <span class="s">assets/images/5.jpg</span>
<span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Something</span><span class="nv"> </span><span class="s">about</span><span class="nv"> </span><span class="s">this</span><span class="nv"> </span><span class="s">post</span><span class="nv"> </span><span class="s">here"</span>
<span class="nn">---</span></code>
</pre>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</div>

<h4>YAML Post Example:</h4>
<div class="language-markdown highlighter-rouge">
    <div class="highlight">
        <table class="rouge-table">
            <tbody>
                <tr>
                    <td class="rouge-gutter gl"></td>
                    <td class="rouge-code">
<pre><code><span class="nn">---</span>
<span class="na">layout</span><span class="pi">:</span> <span class="s">page</span>
<span class="na">title</span><span class="pi">:</span> <span class="s">Kothing Template for Jekyll</span>
<span class="na">comments</span><span class="pi">:</span> <span class="no">true</span>
<span class="nn">---</span>
</code></pre>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</div>

<h4>Rating</h4>
<div class="language-markdown highlighter-rouge">
    <div class="highlight">
        <table class="rouge-table">
            <tbody>
                <tr>
                    <td class="rouge-gutter gl"></td>
                    <td class="rouge-code">
<pre><code><span class="nn">---</span>
<span class="na">layout</span><span class="pi">:</span> <span class="s">post</span>
<span class="na">title</span><span class="pi">:</span>  <span class="s2">"</span><span class="s">We</span><span class="nv"> </span><span class="s">all</span><span class="nv"> </span><span class="s">wait</span><span class="nv"> </span><span class="s">for</span><span class="nv"> </span><span class="s">summer"</span>
<span class="na">author</span><span class="pi">:</span> <span class="s">john</span>
<span class="na">categories</span><span class="pi">:</span> <span class="pi">[</span> <span class="nv">Jekyll</span><span class="pi">,</span> <span class="nv">tutorial</span> <span class="pi">]</span>
<span class="na">image</span><span class="pi">:</span> <span class="s">assets/images/5.jpg</span>
<span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Something</span><span class="nv"> </span><span class="s">about</span><span class="nv"> </span><span class="s">this</span><span class="nv"> </span><span class="s">post</span><span class="nv"> </span><span class="s">here"</span>
<span class="na">rating</span><span class="pi">:</span> <span class="s">4.5</span>
<span class="nn">---</span>
</code></pre>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</div>

<h4>Lazy Load Images</h4>

<p>Enable this option by editing <code class="highlighter-rouge">_config.yml</code>.</p>
<div class="language-markdown highlighter-rouge">
    <div class="highlight">
        <table class="rouge-table">
            <tbody>
                <tr>
                    <td class="rouge-gutter gl"></td>
                    <td class="rouge-code">
<pre><code><span class="gh"># Lazy Images ("enabled" or "disabled")</span>
lazyimages: "enabled"
</code></pre>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</div>

<h4>Table of Contents</h4>
<div class="language-markdown highlighter-rouge">
    <div class="highlight">
        <table class="rouge-table">
            <tbody>
                <tr>
                    <td class="rouge-gutter gl"></td>
                    <td class="rouge-code">
<pre><code><span class="nn">---</span>
<span class="na">layout</span><span class="pi">:</span> <span class="s">post</span>
<span class="na">title</span><span class="pi">:</span>  <span class="s2">"</span><span class="s">Education</span><span class="nv"> </span><span class="s">must</span><span class="nv"> </span><span class="s">also</span><span class="nv"> </span><span class="s">train</span><span class="nv"> </span><span class="s">one</span><span class="nv"> </span><span class="s">for</span><span class="nv"> </span><span class="s">quick,</span><span class="nv"> </span><span class="s">resolute</span><span class="nv"> </span><span class="s">and</span><span class="nv"> </span><span class="s">effective</span><span class="nv"> </span><span class="s">thinking."</span>
<span class="na">author</span><span class="pi">:</span> <span class="s">john</span>
<span class="na">categories</span><span class="pi">:</span> <span class="pi">[</span> <span class="nv">Jekyll</span><span class="pi">,</span> <span class="nv">tutorial</span> <span class="pi">]</span>
<span class="na">image</span><span class="pi">:</span> <span class="s">assets/images/3.jpg</span>
<span class="na">beforetoc</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Markdown</span><span class="nv"> </span><span class="s">editor</span><span class="nv"> </span><span class="s">is</span><span class="nv"> </span><span class="s">a</span><span class="nv"> </span><span class="s">very</span><span class="nv"> </span><span class="s">powerful</span><span class="nv"> </span><span class="s">thing.</span>
<span class="na">toc</span><span class="pi">:</span> <span class="no">true</span>
<span class="nn">---</span>
</code></pre>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</div>

<p><code class="highlighter-rouge">beforetoc</code> adds a paragraph before the TOC is displayed. <a href="https://kothing.github.io/education/">Demo</a></p>

<h4>Questions or bug reports?</h4>

<p>Head over to our <a href="https://github.com/kothing/kothing-theme">Github repository</a>!</p>

</div>

<div class="col-md-4">

<div class="sticky-top sticky-top-80">
<h5>Buy me a coffee</h5>

<p>Thank you for your support! Your donation helps me to maintain and improve <a target="_blank" href="https://github.com/kothing/kothing-theme">Kothing <i class="fab fa-github"></i></a>.</p>

<a target="_blank" href="https://www.paypal.me/chzng" class="btn btn-danger">Buy me a coffee</a> <a target="_blank" href="https://kothing.github.io/about" class="btn btn-warning">Documentation</a>

</div>
</div>
</div>
