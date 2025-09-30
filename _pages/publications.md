---
layout: page
permalink: /publications/
title: Publications
nav: true
nav_order: 4
description: Complete list of publications from the Vision and Autonomy Intelligence Lab at UCLA.
---


Click the buttons below to view our work by category.
<!-- Custom Tag Filter Buttons -->
<div class="publication-tags-container mb-4">
  <div class="tag-buttons">
    <button class="tag-btn active" data-tag="all">All</button>
    <!-- For now, we'll add common research area tags manually -->
    <button class="tag-btn" data-tag="Robotics">Robotics</button>
    <button class="tag-btn" data-tag="Simulator">Simulator</button>
    <button class="tag-btn" data-tag="Autonomous Driving">Autonomous Driving</button>
    <button class="tag-btn" data-tag="Generative Model">Generative Model</button>
        <button class="tag-btn" data-tag="Computer Vision">Computer Vision</button>

  </div>
</div>

<div class="publications">

{% bibliography %}

</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  const tagButtons = document.querySelectorAll('.tag-btn');
  const publications = document.querySelectorAll('.publications ol.bibliography li');

  tagButtons.forEach(button => {
    button.addEventListener('click', function() {
      // Remove active class from all buttons
      tagButtons.forEach(btn => btn.classList.remove('active'));
      // Add active class to clicked button
      this.classList.add('active');
      
      const selectedTag = this.getAttribute('data-tag');
      
      publications.forEach(publication => {
        const publicationDiv = publication.querySelector('div[data-tags]');
        
        if (selectedTag === 'all') {
          publication.style.display = 'block';
        } else if (publicationDiv) {
          const tags = publicationDiv.getAttribute('data-tags') || '';
          const abbr = publicationDiv.getAttribute('data-abbr') || '';
          
          // Check if publication has the selected tag in its tags field or abbr field
          const hasTag = tags.toLowerCase().includes(selectedTag.toLowerCase()) ||
                        abbr.toLowerCase().includes(selectedTag.toLowerCase());
          
          if (hasTag) {
            publication.style.display = 'block';
          } else {
            publication.style.display = 'none';
          }
        } else {
          publication.style.display = 'none';
        }
      });
      
      // Hide empty year sections
      hideEmptyYearSections();
    });
  });
  
  // Initialize year sections visibility on page load
  hideEmptyYearSections();
  
  function hideEmptyYearSections() {
    // Try different possible selectors for year headers
    const yearSections = document.querySelectorAll('.publications h2, .publications h3, .publications .year');
    
    // Debug: log what we found
    console.log('Found year sections:', yearSections.length);
    
    yearSections.forEach(yearSection => {
      // Check if this looks like a year header (contains 4 digits)
      const yearText = yearSection.textContent.trim();
      const isYearHeader = /\b(19|20)\d{2}\b/.test(yearText);
      
      console.log('Checking element:', yearText, 'isYearHeader:', isYearHeader);
      
      if (!isYearHeader) return; // Skip if not a year header
      
      let nextElement = yearSection.nextElementSibling;
      let hasVisiblePublications = false;
      
      // Check all publications under this year until we hit the next year section or end
      while (nextElement) {
        // If we hit another year header, stop
        if ((nextElement.tagName === 'H2' || nextElement.tagName === 'H3') && 
            /\b(19|20)\d{2}\b/.test(nextElement.textContent)) {
          break;
        }
        
        // Check if this is a publication list
        if (nextElement.tagName === 'OL' && nextElement.classList.contains('bibliography')) {
          const publications = nextElement.querySelectorAll('li');
          let visibleCount = 0;
          publications.forEach(pub => {
            if (pub.style.display !== 'none') {
              hasVisiblePublications = true;
              visibleCount++;
            }
          });
          console.log('Year', yearText, 'has', visibleCount, 'visible publications out of', publications.length);
          break;
        }
        
        nextElement = nextElement.nextElementSibling;
      }
      
      // Hide/show year section based on whether it has visible publications
      if (hasVisiblePublications) {
        yearSection.style.display = 'block';
        console.log('Showing year:', yearText);
      } else {
        yearSection.style.display = 'none';
        console.log('Hiding year:', yearText);
      }
    });
  }
});
</script>