---
# Leave the homepage title empty to use the site title
title: ''
summary: ''
date: 2026-01-05
type: landing

design:
  # Default section spacing
  spacing: '0'

sections:
  # Developer Hero - Gradient background with name, role, social, and CTAs
  - block: dev-hero
    id: hero
    content:
      username: me
      greeting: "Hi, I'm"
      show_status: true
      show_scroll_indicator: true
      # typewriter:
      #   enable: true
      #   prefix: "I build"
      #   strings:
      #     - "full-stack web apps"
      #     - "scalable APIs"
      #     - "beautiful UIs"
      #     - "open source tools"
      #   type_speed: 70
      #   delete_speed: 40
      #   pause_time: 2500
      cta_buttons:
        - text: View My Work
          url: "#projects"
          icon: arrow-down
        - text: Get In Touch
          url: "#contact"
          icon: envelope
    design:
      style: centered
      avatar_shape: circle
      animations: true
      background:
        color:
          light: "#f9f6f0"
          dark: "#f9f6f0"
      spacing:
        padding: ["1rem", "0", "1em", "0"]


  
  # Filterable Portfolio - Alpine.js powered project filtering
  - block: portfolio
    id: projects
    content:
      title: "Featured Projects"
      subtitle: "A selection of my recent work"
      count: 0
      filters:
        folders:
          - projects
      # buttons:
      #   - name: All
      #     tag: '*'
      #   - name: Full-Stack
      #     tag: Full-Stack
      #   - name: Frontend
      #     tag: Frontend
      #   - name: Backend
      #     tag: Backend
      # default_button_index: 0
      # Archive link auto-shown if more projects exist than 'count' above
      # archive:
      #   enable: false  # Set to false to explicitly hide
      #   text: "Browse All"  # Customize text
      #   link: "/work/"  # Custom URL
    design:
      columns: '3'
      background:
        color:
          light: "#f9f6f0"
          dark: "#f9f6f0"
      spacing:
        padding: ["3.5rem", "0", "2rem", "0"]
  

  - block: markdown
    id: publications
    content:
      title: Publications & Presentations
      text: |-
        ### Publications

        **Self-Guided Integrated Gradient Method for Attribution**  
        <u>**S.Henry**</u>, A. Ruget, S. Scholes, J. Leach  
        *CVPR 2026 — Findings Track*, Denver, CO, June 2026  
        [Preprint](https://www.techrxiv.org/doi/full/10.36227/techrxiv.175037106.61341481) · [Code](https://github.com/HWQuantum/SIGMA)

        ---

        ### Presentations

        #### Talks

        **A Self-Guided Approach to Integrated Gradients**  
        <u>**S.Henry**</u>  
        *Turing Connections Day*, Alan Turing Institute, Edinburgh, March 2026

        #### Posters

        **Attribution Analysis for Interpretable Machine Learning**  
        <u>**S. Henry**</u>,
        *AI for Science Conference*, The Royal Society, London, March 2026
    design:
      columns: '1'
      background:
        color:
          light: "#B9D0D4"
          dark: "#B9D0D4"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]

  # Experience Timeline
  - block: resume-experience
    id: experience
    content:
      title: Experience
      date_format: Jan 2006
      items:
        - title: Senior Software Engineer
          company: Tech Corp
          company_url: ''
          company_logo: ''
          location: San Francisco, CA
          date_start: '2023-01-01'
          date_end: ''
          description: |2-
            * Lead development of microservices architecture serving 1M+ users
            * Improved API response time by 40% through optimization
            * Mentored team of 5 junior developers
            * Tech stack: React, Node.js, PostgreSQL, AWS
        - title: Full-Stack Developer
          company: Startup Inc
          company_url: ''
          company_logo: ''
          location: Remote
          date_start: '2021-06-01'
          date_end: '2022-12-31'
          description: |2-
            * Built and deployed 3 production applications from scratch
            * Implemented CI/CD pipeline reducing deployment time by 60%
            * Collaborated with design team on UI/UX improvements
            * Tech stack: Next.js, Express, MongoDB, Docker
        - title: Junior Developer
          company: Web Agency
          company_url: ''
          company_logo: ''
          location: New York, NY
          date_start: '2020-01-01'
          date_end: '2021-05-31'
          description: |2-
            * Developed client websites using modern web technologies
            * Maintained and updated legacy codebases
            * Participated in code reviews and agile ceremonies
            * Tech stack: React, WordPress, PHP, MySQL
    design:
      columns: '1'
      background:
        color:
          light: "#f9f6f0"
          dark: "#f9f6f0"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]
  
  
  # Contact Section
  - block: contact-info
    id: contact
    content:
      title: Get In Touch
      text: |-
        I'm always interested in hearing about new projects and opportunities.
        Feel free to reach out!
      email: sjh9@hw.ac.uk
      autolink: true
    design:
      columns: '1'
      background:
        color:
          light: "#B9D0D4"
          dark: "#B9D0D4"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]
  
#   # CTA Card
#   - block: cta-card
#     content:
#       title: "Open to Opportunities"
#       text: |-
#         I'm currently looking for **senior engineering** or **tech lead** roles.
        
#         Let's connect and discuss how I can help your team.
#       button:
#         text: 'Download Resume'
#         url: uploads/resume.pdf
#         new_tab: true
#     design:
#       card:
#         # Light mode: soft pastel theme gradient | Dark mode: rich deep gradient
#         css_class: 'bg-gradient-to-br from-primary-200 via-primary-100 to-secondary-200 dark:from-primary-600 dark:via-primary-700 dark:to-secondary-700'
#         text_color: dark
#       background:
#         color:
#           light: "#d9ebf9"
#           dark: "#d9ebf9"
#       spacing:
#         padding: ["4rem", "0", "6rem", "0"]
---
