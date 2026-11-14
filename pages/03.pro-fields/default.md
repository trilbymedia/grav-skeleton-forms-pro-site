---
title: 'Every pro field'
menu: 'Pro fields'
process:
  markdown: true
  twig: false
---

# Every pro field

All eleven Forms Pro fields in one form: **name**, **phone**, **address**, **recipients**, **date picker**, **time picker**, **slider**, **calculation**, **repeater**, **rich text** and **rating**. The help text under each one names its field type. They sit in the same palette as the Form plugin's 38 fields, so one builder covers them all.

> [!NOTE]
> The date and time pickers use Flatpickr when a theme or plugin loads it. This site does not, so they fall back to your browser's own date and time inputs. The server still checks the limits either way.

[forms-pro name="studio-visit" /]

## How it's built

<details class="demo-yaml">
<summary>Show the YAML <code>studio-visit.yaml</code></summary>
<pre><code class="language-yaml"># user/data/forms-pro/forms/studio-visit.yaml
title: 'Plan a studio visit'
type: forms-pro
inline_errors: true
fields:
  honeypot:
    type: honeypot
  visitor:
    type: pro-name
    label: 'Your name'
    description: 'pro-name: each part is stored on its own, so an email can say "Hi Maya".'
    subfields:
      prefix: true
      first: true
      middle: false
      last: true
      suffix: false
    placeholder_first: First
    placeholder_last: Last
    validate:
      required: true
  mobile:
    type: pro-phone
    label: Mobile
    description: 'pro-phone: a number with a country code picker.'
    show_country_code: true
    default_country: US
    placeholder: '555 010 4477'
  company_address:
    type: pro-address
    label: 'Company address'
    description: 'pro-address: street, city, state, postal code and country, here in the inline layout.'
    layout: inline
    subfields:
      street: true
      street2: false
      city: true
      state: true
      zip: true
      country: true
    placeholder_street: '400 Market Street'
    placeholder_city: City
    placeholder_state: State
    placeholder_zip: 'Postal code'
  host:
    type: pro-recipients
    label: 'Who are you coming to see?'
    description: 'pro-recipients: each choice maps to an email address that stays on the server.'
    display: select
    placeholder: 'Choose a team'
    recipients:
      design:
        label: 'Design team'
        email: design@example.com
      dev:
        label: 'Development team'
        email: dev@example.com
      founders:
        label: Founders
        email: founders@example.com
    validate:
      required: true
  visit_date:
    type: pro-datepicker
    label: 'Visit date'
    description: 'pro-datepicker: today or later, weekdays only.'
    format: 'F j, Y'
    min_date: today
    disable_weekends: true
    validate:
      required: true
  arrival:
    type: pro-timepicker
    label: 'Arrival time'
    description: 'pro-timepicker: 15 minute steps between 9am and 5pm.'
    minute_increment: 15
    min_time: '09:00'
    max_time: '17:00'
  group_size:
    type: pro-slider
    label: 'How many people are coming?'
    description: 'pro-slider: a range with its value shown as you drag.'
    min: 1
    max: 12
    step: 1
    default: 2
    suffix: ' people'
  parking:
    type: pro-calculation
    label: 'Parking passes we will reserve'
    description: 'pro-calculation: one pass for every three people, worked out live.'
    formula: 'CEIL(group_size / 3)'
    decimals: 0
    formula_display: true
  guests:
    type: pro-repeater
    label: 'Guest list'
    description: 'pro-repeater: add, remove and drag rows to reorder them.'
    min: 1
    max: 12
    add_label: 'Add a guest'
    item_label: Guest
    sortable: true
    fields:
      guest_name:
        type: text
        label: Name
        validate:
          required: true
      guest_role:
        type: text
        label: 'Role'
        placeholder: 'Marketing lead'
  agenda:
    type: pro-richtext
    label: 'What would you like to cover?'
    description: 'pro-richtext: a small editor with no outside library. The HTML is cleaned on the server.'
    toolbar: standard
    min_height: 140px
  excitement:
    type: pro-rating
    label: 'How excited are you about the coffee?'
    description: 'pro-rating: stars, hearts, thumbs or circles, with half ratings if you want them.'
    max: 5
    icon: heart
    show_value: true
buttons:
  - type: submit
    value: 'Request a visit'
process:
  - message: 'See you soon, {{ form.value.visitor.first }}! This is a demo, so no visit was booked and nothing was stored.'
  - reset: true</code></pre>
</details>
