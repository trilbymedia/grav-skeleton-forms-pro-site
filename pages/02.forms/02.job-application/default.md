---
title: 'Multi-step job application'
menu: 'Multi-step application'
process:
  markdown: true
  twig: false
---

# Multi-step job application

A four-step application with numbered step circles. Each step is checked before **Next** lets you move on, step three has a required file upload, and a question on step two only appears for studio or hybrid work. When you send it, a **Display Page** action shows a thank-you page in place of the form.

**Try this:**

- Press **Next** on an empty step to see the checks.
- On step two, pick *In the Bristol studio* and a relocation question appears. Switch to *Fully remote* and it goes away again, and its answer is dropped.
- Upload any PDF on step three. **Next** waits until the upload has finished. Uploads go to a temporary folder outside the web root that is emptied every hour.
- The circles are clickable. Jumping ahead checks every step you skip.

[forms-pro name="job-application" /]

## How it's built

A multi-step form is a `pro-steps` field holding one `pro-step` per page. Forms Pro has three progress styles: numbered circles (this form, `progress: steps`), a progress bar (the [workshop booking](/forms/workshop-booking)) and tabs (the [feedback survey](/forms/feedback-survey)). Answers are stored flat, so the step a field sits in never matters to notifications or entries.

<details class="demo-yaml">
<summary>Show the YAML <code>job-application.yaml</code></summary>
<pre><code class="language-yaml"># user/data/forms-pro/forms/job-application.yaml
title: 'Join Lumen &amp; Pine'
type: forms-pro
inline_errors: true
fields:
  steps:
    type: pro-steps
    progress: steps
    show_step_numbers: true
    fields:
      step_about:
        type: pro-step
        title: 'About you'
        description: 'The basics, so we know who to reply to.'
        fields:
          honeypot:
            type: honeypot
          full_name:
            type: pro-name
            label: 'Full name'
            subfields:
              prefix: false
              first: true
              middle: false
              last: true
              suffix: false
            placeholder_first: First
            placeholder_last: Last
            validate:
              required: true
          email:
            type: email
            label: Email
            placeholder: you@example.com
            validate:
              required: true
          phone:
            type: pro-phone
            label: Phone
            show_country_code: true
            default_country: GB
            placeholder: '07700 900123'
          location:
            type: text
            label: 'Where are you based?'
            placeholder: 'City, country'
      step_role:
        type: pro-step
        title: 'The role'
        description: 'Tell us what you would like to do with us.'
        fields:
          role:
            type: select
            label: 'Which role are you applying for?'
            placeholder: 'Choose a role'
            options:
              frontend: 'Front-end developer'
              designer: 'Product designer'
              pm: 'Project manager'
            validate:
              required: true
          work_mode:
            type: radio
            label: 'Where would you like to work?'
            options:
              studio: 'In the Bristol studio'
              hybrid: 'Hybrid, two days in the studio'
              remote: 'Fully remote'
            validate:
              required: true
          relocate:
            type: radio
            label: 'Would you need help relocating?'
            description: 'This question appears because you picked the studio or hybrid.'
            options:
              'no': 'No, I already live nearby'
              'yes': 'Yes, I would be moving'
            conditions:
              - action: show
                logic: any
                rules:
                  - field: work_mode
                    operator: is
                    value: studio
                  - field: work_mode
                    operator: is
                    value: hybrid
          experience:
            type: pro-slider
            label: 'Years of experience'
            min: 0
            max: 20
            step: 1
            default: 3
            suffix: ' yrs'
          start_date:
            type: pro-datepicker
            label: 'Earliest start date'
            format: Y-m-d
            min_date: today
      step_work:
        type: pro-step
        title: 'Your work'
        description: 'A CV is required. Uploads in this demo go to a temporary folder that is emptied every hour.'
        fields:
          cv:
            type: file
            label: 'CV or resume'
            description: 'PDF or Word, up to 5 MB.'
            multiple: false
            limit: 1
            filesize: 5
            accept:
              - application/pdf
              - application/msword
              - application/vnd.openxmlformats-officedocument.wordprocessingml.document
            destination: 'tmp://forms-pro-demo/uploads'
            random_name: true
            avoid_overwriting: true
            validate:
              required: true
          portfolio:
            type: url
            label: 'Portfolio or GitHub'
            placeholder: 'https://'
          cover_note:
            type: pro-richtext
            label: 'Why Lumen &amp; Pine?'
            toolbar: basic
            min_height: 140px
            placeholder: 'A few lines is plenty.'
      step_finish:
        type: pro-step
        title: 'Last check'
        description: 'Nearly done.'
        fields:
          heard_about:
            type: select
            label: 'How did you hear about us?'
            placeholder: 'Choose one'
            options:
              friend: 'A friend or colleague'
              jobboard: 'A job board'
              social: 'Social media'
              event: 'A meetup or conference'
              other: Somewhere else
          consent:
            type: checkbox
            label: 'I understand this is a demo and nothing I send is kept.'
            validate:
              required: true
buttons:
  - type: submit
    value: 'Send application'
process:
  - upload: true
  - display: thanks</code></pre>
</details>
