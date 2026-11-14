---
title: 'Workshop booking with a repeater'
menu: 'Booking with a repeater'
process:
  markdown: true
  twig: false
---

# Workshop booking with a repeater

A three-step booking with a progress bar. The total updates as you change the ticket and the number of seats, with 10% off for five seats or more. Step two has a **Repeater** for the people coming: add a row for each person, drag rows to reorder them and remove the ones you don't need.

**Try this:**

- Set **Seats** to 5 and watch the discount apply. The formula is shown under the total.
- On step two, add a few attendees and drag one by its handle. Each row's name is required, and an error says which row it is on.
- The total is worked out again on the server when the form is sent, so changing it with the browser's developer tools changes nothing.

[forms-pro name="workshop-booking" /]

## How it's built

The ticket options use their price as the key (`'149'`, `'89'`), so the formula can read the price straight from the choice. The repeater's fields are ordinary fields, and its answer is stored as a list with one entry per row.

<details class="demo-yaml">
<summary>Show the YAML <code>workshop-booking.yaml</code></summary>
<pre><code class="language-yaml"># user/data/forms-pro/forms/workshop-booking.yaml
title: 'Book a workshop'
type: forms-pro
inline_errors: true
fields:
  steps:
    type: pro-steps
    progress: bar
    fields:
      step_session:
        type: pro-step
        title: 'Pick a session'
        description: 'One day, hands-on, laptops required. Groups of five or more get 10% off.'
        fields:
          honeypot:
            type: honeypot
          workshop:
            type: select
            label: Workshop
            placeholder: 'Choose a workshop'
            options:
              flatfile: 'Flat-file sites in a day'
              forms: 'Forms that people finish'
              a11y: 'Accessibility for busy teams'
            validate:
              required: true
          session_date:
            type: radio
            label: Date
            options:
              mar12: 'Thursday 12 March, Bristol'
              apr09: 'Thursday 9 April, Manchester'
              may14: 'Thursday 14 May, online'
            validate:
              required: true
          ticket:
            type: radio
            label: Ticket
            options:
              '149': 'Full day with lunch ($149)'
              '89': 'Morning only ($89)'
            default: '149'
            validate:
              required: true
          seats:
            type: number
            label: Seats
            default: 1
            validate:
              required: true
              min: 1
              max: 12
          total:
            type: pro-calculation
            label: 'Total'
            formula: 'seats * ticket * IF(seats &gt;= 5, 0.9, 1)'
            prefix: '$'
            decimals: 2
            formula_display: true
      step_people:
        type: pro-step
        title: "Who's coming"
        description: 'Add a row for each person, so we can print their name badges.'
        fields:
          attendees:
            type: pro-repeater
            label: Attendees
            min: 1
            max: 12
            add_label: 'Add attendee'
            item_label: Attendee
            sortable: true
            fields:
              attendee_name:
                type: text
                label: Name
                validate:
                  required: true
              attendee_email:
                type: email
                label: Email
              dietary:
                type: select
                label: 'Dietary needs'
                options:
                  none: 'None'
                  vegetarian: Vegetarian
                  vegan: Vegan
                  gluten_free: 'Gluten free'
      step_billing:
        type: pro-step
        title: Billing
        description: 'Where should the invoice go?'
        fields:
          billing_name:
            type: pro-name
            label: 'Billing contact'
            subfields:
              prefix: false
              first: true
              middle: false
              last: true
              suffix: false
            validate:
              required: true
          billing_email:
            type: email
            label: 'Billing email'
            placeholder: accounts@example.com
            validate:
              required: true
          company:
            type: text
            label: 'Company (optional)'
          billing_address:
            type: pro-address
            label: 'Billing address'
            layout: inline
            subfields:
              street: true
              street2: true
              city: true
              state: true
              zip: true
              country: true
            placeholder_street: '12 Harbour Lane'
            validate:
              required: true
buttons:
  - type: submit
    value: 'Book my seats'
process:
  - message: 'Booked, {{ form.value.billing_name.first }}! Well, almost: this is a demo, so no seats were held and nothing was stored.'
  - reset: true</code></pre>
</details>
