---
layout: page
permalink: /publications/
title: Publications
nav: true
nav_order: 4
description:  
---
<style>
  /* Make year headers more visible */
  .publications h2.year,
  .publications h2,
  .publications h3.year,
  .publications h3 {
    color: var(--global-theme-color) !important;
    font-weight: 600 !important;
    font-size: 2.0rem !important;
    margin-top: 0.0rem !important;
    margin-bottom: 0.0rem !important;
    position: relative;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  
  /* Style for "Before UCLA" badge - matches year format */
  .year-badge {
    color: var(--global-theme-color) !important;
    font-weight: 600 !important;
    font-size: 2.0rem !important;
  }
</style>

Click the buttons below to view our work by category. Please see [Google Scholar](https://scholar.google.com/citations?hl=en&user=9D4aG8AAAAAJ&view_op=list_works&sortby=pubdate) for more recent works and arXiv papers.
<!-- Custom Tag Filter Buttons -->
<div class="publication-tags-container mb-4">
  <div class="tag-buttons">
    <button class="tag-btn active" data-tag="all">All</button>
    <!-- For now, we'll add common research area tags manually -->
    <button class="tag-btn" data-tag="Computer Vision">Computer Vision</button>
    <button class="tag-btn" data-tag="Robotics">Robotics</button>
    <button class="tag-btn" data-tag="Simulation">Simulation</button>
    <button class="tag-btn" data-tag="Autonomy">Autonomy</button>
    <button class="tag-btn" data-tag="Generative Models">Generative Models</button>
    <button class="tag-btn" data-tag="Others">Others</button>

  </div>
</div>

<div class="publications">

{% bibliography %}

</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  const tagButtons = document.querySelectorAll('.tag-btn');
  const publications = document.querySelectorAll('.publications ol.bibliography li');

  // Add "Before UCLA" badge only to year 2022
  const yearSections = document.querySelectorAll('.publications h2, .publications h3, .publications .year');
  yearSections.forEach(yearSection => {
    const yearText = yearSection.textContent.trim();
    const yearMatch = yearText.match(/\b(19|20)\d{2}\b/);
    
    if (yearMatch) {
      const year = parseInt(yearMatch[0]);
      if (year === 2022) {
        // Add badge if it doesn't already exist
        if (!yearSection.querySelector('.year-badge')) {
          const badge = document.createElement('span');
          badge.className = 'year-badge';
          badge.textContent = 'Before UCLA';
          yearSection.style.position = 'relative';
          yearSection.classList.add('has-badge');
          yearSection.insertBefore(badge, yearSection.firstChild);
        }
      }
    }
  });

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