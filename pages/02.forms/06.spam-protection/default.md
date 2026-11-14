---
title: 'Spam protection'
menu: 'Spam protection'
process:
  markdown: true
  twig: false
---

# Spam protection

Three ways to keep bots out, all running on this server with no account at a third party. Forms Pro also supports Google reCAPTCHA and Cloudflare Turnstile, which need keys from those providers, so they are not shown here.

Every challenge has two halves: the field that shows it, and a validation action that checks the answer on the server before anything else runs. Pick a challenge on the form's **Settings** tab and the builder adds both.

## Honeypot

A field hidden from people. Bots that fill in every field fill it in too, and their submission is refused. Real visitors never see it, so it costs them nothing. Every form on this site has one.

[forms-pro name="newsletter-honeypot" /]

<details class="demo-yaml">
<summary>Show the YAML <code>newsletter-honeypot.yaml</code></summary>
<pre><code class="language-yaml"># user/data/forms-pro/forms/newsletter-honeypot.yaml
title: 'Studio newsletter'
type: forms-pro
inline_errors: true
fields:
  honeypot:
    type: honeypot
  email:
    type: email
    label: 'Email address'
    placeholder: you@example.com
    validate:
      required: true
buttons:
  - type: submit
    value: Subscribe
process:
  - message: 'You are on the list! (Not really: this is a demo, so the address was not kept.)'
  - reset: true</code></pre>
</details>

## Basic captcha

A small sum drawn as an image on this server. Answer it wrong and the form comes back with an error, and nothing runs. The **refresh** button draws a new sum.

[forms-pro name="question-basic-captcha" /]

<details class="demo-yaml">
<summary>Show the YAML <code>question-basic-captcha.yaml</code></summary>
<pre><code class="language-yaml"># user/data/forms-pro/forms/question-basic-captcha.yaml
title: 'Ask a quick question'
type: forms-pro
inline_errors: true
fields:
  honeypot:
    type: honeypot
  email:
    type: email
    label: 'Your email'
    placeholder: you@example.com
    validate:
      required: true
  question:
    type: textarea
    label: 'Your question'
    rows: 3
    validate:
      required: true
  challenge:
    type: basic-captcha
    label: 'Solve the sum'
    placeholder: 'Answer'
    captcha_type: math
    validate:
      required: true
buttons:
  - type: submit
    value: 'Ask away'
process:
  - basic-captcha: true
  - message: 'Correct answer, thanks! This is a demo, so your question was not stored.'
  - reset: true</code></pre>
</details>

## Cap proof of work

Tick the box and your browser solves a small calculation, which takes a moment for a person and a lot of work for a bot sending thousands of forms. Cap can also run invisibly, with no box at all. It runs on your own server, so no visitor data goes to a third party.

[forms-pro name="callback-cap" /]

<details class="demo-yaml">
<summary>Show the YAML <code>callback-cap.yaml</code></summary>
<pre><code class="language-yaml"># user/data/forms-pro/forms/callback-cap.yaml
title: 'Request a call back'
type: forms-pro
inline_errors: true
fields:
  honeypot:
    type: honeypot
  first_name:
    type: text
    label: 'First name'
    validate:
      required: true
  phone:
    type: pro-phone
    label: 'Phone number'
    show_country_code: true
    default_country: US
    placeholder: '555 010 4477'
    validate:
      required: true
  cap:
    type: cap
    label: 'Checking you are human'
    mode: checkbox
buttons:
  - type: submit
    value: 'Call me back'
process:
  - cap: true
  - message: 'Thanks, {{ form.value.first_name }}! Your browser solved the proof of work. This is a demo, so nobody will call.'
  - reset: true</code></pre>
</details>
