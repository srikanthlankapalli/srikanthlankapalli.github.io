---
layout: default
title: Home
---

<style>
  body {
    min-height: 100vh;
  }

  .page-content {
    min-height: calc(100vh - 120px);
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .home-shell {
    width: 100%;
    min-height: 100vh;
    margin: 0;
    padding: 0 0 3rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .home-header {
    margin-bottom: 1.5rem;
    text-align: center;
  }

  .home-header h1 {
    margin-bottom: 0.4rem;
  }

  .search-box {
    margin: 1rem auto 1.5rem;
    max-width: 620px;
  }

  .search-box input {
    width: 100%;
    padding: 0.75rem 0.9rem;
    border: 1px solid #d0d7de;
    border-radius: 8px;
    font-size: 1rem;
    box-sizing: border-box;
  }

  .panel {
    border: 1px solid #e5e5e5;
    border-radius: 10px;
    padding: 1rem 1.25rem;
    background: #fafafa;
    width: 100%;
    margin: 0 auto;
  }

  .panel h2 {
    margin-top: 0;
  }

  .recent-posts {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .post-item {
    padding: 0.7rem 0;
    border-bottom: 1px solid #ececec;
  }

  .post-item:last-child {
    border-bottom: none;
  }

  .post-item a {
    font-weight: 600;
    text-decoration: none;
  }

  .post-item small {
    display: block;
    color: #666;
    margin-top: 0.2rem;
  }

  .tag-list {
    display: inline-flex;
    flex-wrap: wrap;
    gap: 0.35rem;
  }

  .category-tag {
    display: inline-block;
    color: #1f4f99;
    text-decoration: none;
    background: #eef3ff;
    border: 1px solid #dfe8ff;
    border-radius: 999px;
    padding: 0.15rem 0.55rem;
    font-size: 0.8rem;
    line-height: 1.5;
  }

  .category-tag:hover {
    text-decoration: underline;
  }
</style>

<div class="home-shell">

  <div class="home-header">
    <h1>Notes by Srikanth Lankapalli</h1>
    <p>Thoughts, learning notes, and things I build.</p>
  </div>

  <div class="search-box">
    <label for="post-search">Search posts</label>
    <input id="post-search" type="search" placeholder="Search by title, category, or keyword..." aria-label="Search posts" />
  </div>

  <section class="panel">
    <h2>Recent posts</h2>
    <ul class="recent-posts">
      {% assign recent_posts = site.posts | slice: 0, 6 %}
      {% for post in recent_posts %}
        <li class="post-item" data-title="{{ post.title | escape }}" data-category="{% for category in post.categories %}{{ category }} {% endfor %}" data-content="{{ post.content | strip_html | normalize_whitespace | escape }}">
          <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
          <small>
            {{ post.date | date: "%B %-d, %Y" }}
            {% if post.categories.size > 0 %}
              ·
              <span class="tag-list">
                {% for category in post.categories %}
                  <a href="#" class="category-tag" data-category="{{ category | escape }}">{{ category }}</a>
                {% endfor %}
              </span>
            {% endif %}
          </small>
        </li>
      {% endfor %}
    </ul>
  </section>
</div>

<script>
  const searchInput = document.getElementById('post-search');
  const posts = Array.from(document.querySelectorAll('.post-item'));

  function applyFilter(query) {
    const normalized = (query || '').trim().toLowerCase();

    posts.forEach(function (post) {
      const text = (post.dataset.title || '') + ' ' + (post.dataset.category || '') + ' ' + (post.dataset.content || '');
      const isMatch = !normalized || text.toLowerCase().includes(normalized);
      post.style.display = isMatch ? '' : 'none';
    });
  }

  if (searchInput) {
    searchInput.addEventListener('input', function () {
      applyFilter(this.value);
    });
  }

  document.addEventListener('click', function (event) {
    const tag = event.target.closest('.category-tag');
    if (!tag) return;

    event.preventDefault();
    const category = tag.dataset.category || '';
    if (searchInput) {
      searchInput.value = category;
    }
    applyFilter(category);
  });
</script>
