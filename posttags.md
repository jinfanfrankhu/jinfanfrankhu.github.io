---
layout: page
title: Post Tags
permalink: /posttags/
---
<!-- Tag buttons will go here -->
<div id="tag-buttons"></div>

<!-- Posts will go here -->
<ul id="tagged-posts"></ul>

<script>
// STEP 1: Generate posts data from Jekyll
const posts = [
  {% for post in site.posts %}
    {
      title: {{ post.title | jsonify }},
      url: {{ post.url | relative_url | jsonify }},
      tags: {{ post.tags | jsonify }}
    },
  {% endfor %}
];

// STEP 2: Count tag frequencies
const tagCounts = {};
posts.forEach(post => {
  post.tags.forEach(tag => {
    tagCounts[tag] = (tagCounts[tag] || 0) + 1;
  });
});

// STEP 3: Create sorted tag buttons
const tagButtonsDiv = document.getElementById('tag-buttons');
const sortedTags = Object.keys(tagCounts).sort((a, b) => tagCounts[b] - tagCounts[a]);

const allButton = document.createElement('button');
allButton.innerText = 'All';
allButton.dataset.tag = 'all';
tagButtonsDiv.appendChild(allButton);

sortedTags.forEach(tag => {
  const btn = document.createElement('button');
  btn.innerText = `${tag} (${tagCounts[tag]})`;
  btn.dataset.tag = tag;
  tagButtonsDiv.appendChild(btn);
});

// STEP 4: Render all posts
const postsList = document.getElementById('tagged-posts');
function renderPosts(filterTag = 'all') {
  postsList.innerHTML = '';
  posts.forEach(post => {
    if (filterTag === 'all' || post.tags.includes(filterTag)) {
      const li = document.createElement('li');
      const a = document.createElement('a');
      a.href = post.url;
      a.textContent = post.title;
      li.appendChild(a);
      postsList.appendChild(li);
    }
  });
}
renderPosts(); // Show all by default

// STEP 5: Hook up button filtering
tagButtonsDiv.addEventListener('click', e => {
  if (e.target.tagName === 'BUTTON') {
    renderPosts(e.target.dataset.tag);
  }
});
</script>
