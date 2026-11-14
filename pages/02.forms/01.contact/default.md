---
title: 'Contact form with recipient routing'
menu: 'Contact with routing'
process:
  markdown: true
  twig: false
---

# Contact form with recipient routing

A contact form where the visitor chooses who should read the message. Each choice in the **Recipients** field has its own email address, and those addresses live in the form definition on the server, so they never appear in the form's HTML: only short keys such as `support` do. (They do appear in the YAML at the bottom of the page, because this demo prints its own source.)

**Try this:**

- Pick *Help with an existing site* or *Billing* and a **Project reference** field appears. That is a rule on the Logic tab that matches *any* of two answers.
- Press **Send message** with nothing filled in to see the errors next to each field.
- The name is a **Name** field, so the first and last name are stored as separate parts. The thank-you message uses just the first name.

[forms-pro name="contact" /]

## How it's built

This is the whole form, as the builder saves it.

<details class="demo-yaml">
<summary>Show the YAML <code>contact.yaml</code></summary>
<pre><code class="language-yaml"># user/data/forms-pro/forms/contact.yaml
title: 'Contact the studio'
type: forms-pro
inline_errors: true
fields:
  honeypot:
    type: honeypot
  full_name:
    type: pro-name
    label: 'Your name'
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
    label: 'Email address'
    placeholder: you@example.com
    validate:
      required: true
  department:
    type: pro-recipients
    label: 'Who should read this?'
    display: radio
    description: 'Each choice has its own inbox. The addresses stay in the form definition on the server.'
    recipients:
      projects:
        label: 'A new project'
        email: projects@example.com
      support:
        label: 'Help with an existing site'
        email: 'support@example.com, oncall@example.com'
      billing:
        label: Billing
        email: accounts@example.com
      press:
        label: 'Press and partnerships'
        email: press@example.com
    validate:
      required: true
  project_ref:
    type: text
    label: 'Project reference'
    placeholder: 'LP-2041'
    description: 'Shown only for support and billing questions.'
    conditions:
      - action: show
        logic: any
        rules:
          - field: department
            operator: is
            value: support
          - field: department
            operator: is
            value: billing
  message:
    type: textarea
    label: Message
    rows: 5
    placeholder: 'How can we help?'
    minlength: 10
    validate:
      required: true
buttons:
  - type: submit
    value: 'Send message'
process:
  - message: 'Thanks, {{ form.value.full_name.first }}! This is a demo, so nothing was sent or stored. On a real site your message would now be on its way to the inbox behind the choice you picked.'
  - reset: true</code></pre>
</details>

On your own site you would add a **Notifications** action that sends to whichever inbox the visitor picked. The demo leaves it out, so nothing is ever emailed:

```yaml
process:
  - submission: true
  - notify:
      - to: '{{ recipients.department }}'
        subject: 'New message from {{ form.value.full_name.first }}'
        reply_to: '{{ form.value.email }}'
  - message: 'Thanks, we will be in touch.'
```
