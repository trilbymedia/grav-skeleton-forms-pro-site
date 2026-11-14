---
title: 'Quote request with conditional logic'
menu: 'Conditional logic'
process:
  markdown: true
  twig: false
---

# Quote request with conditional logic

Five project types, each asking its own questions. Rules on the **Logic** tab show and hide fields as you answer, and the server runs the same rules when the form arrives: a hidden field can't hold up the form by being required, and anything typed into it before it was hidden is thrown away.

**Try this:**

- Pick each project type in turn and watch the questions change.
- *A website* and *An online shop* share one question about moving content, using a rule that matches *any* of the two. Turn it on for a field asking for the old site's address.
- Choose *As soon as possible* under **When do you need it?** for a note on rush fees. Even a block of text can have rules.
- Tick *We have worked together before* and a required client number appears. Untick it and the form sends without one.

[forms-pro name="project-quote" /]

## How it's built

Rules are stored on the field they show or hide, under `conditions`, so a rule written by hand shows up on the Logic tab and the other way round. There are ten operators: `is`, `is_not`, `contains`, `not_contains`, `starts_with`, `ends_with`, `greater_than`, `less_than`, `is_empty` and `is_not_empty`.

<details class="demo-yaml">
<summary>Show the YAML <code>project-quote.yaml</code></summary>
<pre><code class="language-yaml"># user/data/forms-pro/forms/project-quote.yaml
title: 'Request a project quote'
type: forms-pro
inline_errors: true
fields:
  honeypot:
    type: honeypot
  contact_row:
    type: columns
    fields:
      contact_left:
        type: column
        fields:
          full_name:
            type: text
            label: 'Your name'
            validate:
              required: true
      contact_right:
        type: column
        fields:
          email:
            type: email
            label: Email
            placeholder: you@example.com
            validate:
              required: true
  project_type:
    type: radio
    label: 'What are we building?'
    description: 'Pick one and watch the questions below change.'
    options:
      website: 'A website'
      shop: 'An online shop'
      app: 'A web or mobile app'
      branding: 'Branding'
      other: 'Something else'
    validate:
      required: true
  page_count:
    type: select
    label: 'Roughly how many pages?'
    placeholder: 'Choose a size'
    options:
      small: '1 to 5'
      medium: '6 to 15'
      large: 'More than 15'
    validate:
      required: true
    conditions:
      - action: show
        rules:
          - field: project_type
            operator: is
            value: website
  product_count:
    type: number
    label: 'How many products will you sell?'
    placeholder: '120'
    validate:
      required: true
      min: 1
    conditions:
      - action: show
        rules:
          - field: project_type
            operator: is
            value: shop
  payments:
    type: checkboxes
    label: 'Which payment providers?'
    options:
      stripe: Stripe
      paypal: PayPal
      square: Square
      invoice: 'Bank transfer or invoice'
    conditions:
      - action: show
        rules:
          - field: project_type
            operator: is
            value: shop
  migration:
    type: toggle
    label: 'Moving content from an old site?'
    highlight: 1
    default: 0
    options:
      1: 'Yes'
      0: 'No'
    conditions:
      - action: show
        logic: any
        rules:
          - field: project_type
            operator: is
            value: website
          - field: project_type
            operator: is
            value: shop
  old_url:
    type: url
    label: 'Address of the current site'
    placeholder: 'https://'
    conditions:
      - action: show
        rules:
          - field: migration
            operator: is
            value: '1'
  platforms:
    type: checkboxes
    label: 'Which platforms?'
    options:
      ios: iOS
      android: Android
      web: 'Web app'
    conditions:
      - action: show
        rules:
          - field: project_type
            operator: is
            value: app
  deliverables:
    type: checkboxes
    label: 'What do you need?'
    options:
      logo: Logo
      palette: 'Colour palette'
      type: Typography
      guidelines: 'Brand guidelines'
    conditions:
      - action: show
        rules:
          - field: project_type
            operator: is
            value: branding
  other_details:
    type: textarea
    label: 'Tell us about it'
    rows: 3
    validate:
      required: true
    conditions:
      - action: show
        rules:
          - field: project_type
            operator: is
            value: other
  budget:
    type: select
    label: Budget
    placeholder: 'Choose a range'
    options:
      under5: 'Under $5,000'
      5to15: '$5,000 to $15,000'
      15to40: '$15,000 to $40,000'
      over40: 'Over $40,000'
      unsure: 'Not sure yet'
    validate:
      required: true
  timeline:
    type: select
    label: 'When do you need it?'
    placeholder: 'Choose a timeline'
    options:
      asap: 'As soon as possible'
      quarter: 'In the next three months'
      flexible: 'No fixed date'
  rush_note:
    type: display
    label: 'A note on rush jobs'
    markdown: true
    content: 'Projects that start within two weeks carry a **20% rush fee**, and we will confirm availability before we quote.'
    conditions:
      - action: show
        rules:
          - field: timeline
            operator: is
            value: asap
  existing_client:
    type: checkbox
    label: 'We have worked together before'
  client_ref:
    type: text
    label: 'Your client number'
    placeholder: 'C-1024'
    validate:
      required: true
    conditions:
      - action: show
        rules:
          - field: existing_client
            operator: is
            value: '1'
buttons:
  - type: submit
    value: 'Request my quote'
process:
  - message: 'Thanks, {{ form.value.full_name }}! In this demo nothing is sent or stored. Fields that were hidden by your answers were dropped on the server too.'
  - reset: true</code></pre>
</details>
