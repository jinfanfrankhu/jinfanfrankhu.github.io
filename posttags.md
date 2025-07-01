---
layout: page
title: Post Tags
permalink: /posttags/
---
<div class="tag-hub">
   <!-- Tag buttons will go here -->
   <div id="tag-buttons"></div>

  <!-- Posts will go here -->
  <ul id="tagged-posts"></ul>
</div>

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
  let index = 0;
  posts.forEach(post => {
    if (filterTag === 'all' || post.tags.includes(filterTag)) {
      index++;
      const catIndex = (index - 1) % 4 + 1;

      const li = document.createElement('li');
      li.classList.add('custom-post-item');

      // Create cat bullet image
      const img = document.createElement('img');
      img.src = `/images/cat${catIndex}.png`;
      img.alt = `cat ${catIndex}`;
      img.className = 'cat-bullet';

      // Create post link
      const a = document.createElement('a');
      a.href = post.url;
      a.innerHTML = `<strong>${post.title}</strong>`;

      // Wrap in post content div
      const div = document.createElement('div');
      div.className = 'post-content';
      div.appendChild(a);

      // Combine all
      li.appendChild(img);
      li.appendChild(div);
      postsList.appendChild(li);
    }
  });
}

renderPosts(); // Show all by default

// STEP 5: Hook up button filtering with active styling
tagButtonsDiv.addEventListener('click', e => {
  if (e.target.tagName === 'BUTTON') {
    // remove active class from all buttons
    document.querySelectorAll('#tag-buttons button').forEach(btn => {
      btn.classList.remove('active');
    });

    // add active to clicked one
    e.target.classList.add('active');

    // render posts
    renderPosts(e.target.dataset.tag);
  }
});

</script>
