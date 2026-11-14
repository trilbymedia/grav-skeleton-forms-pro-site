---
title: 'Calculator gallery'
menu: Calculators
process:
  markdown: true
  twig: false
---

# Calculator gallery

Five live calculators, each a Forms Pro form in **calculator mode** (`mode: calculator`). They have no button and run no actions, so nothing you type leaves your browser. Because every visitor gets the same HTML, this page stays in Grav's page cache like any other page.

Formulas are written with field names, arithmetic, comparisons and thirteen functions such as `IF`, `ROUND`, `CEIL`, `POW` and `SUM`. The same formula engine runs in the browser and on the server, so a form that does submit gets every result worked out again where nobody can tamper with it.

<div class="grid demo-calc-row" style="margin-bottom:2.5rem">
<div class="demo-calc">
[forms-pro name="loan-calculator" /]
<details class="demo-yaml">
<summary>Show the YAML <code>loan-calculator.yaml</code></summary>
<pre><code class="language-yaml"># user/data/forms-pro/forms/loan-calculator.yaml
title: 'Loan repayments'
type: forms-pro
mode: calculator
show_title: true
description: 'Monthly repayment for a fixed-rate loan or mortgage.'
fields:
  amount:
    type: number
    label: 'Loan amount ($)'
    default: 250000
    validate:
      min: 0
      step: any
  rate:
    type: number
    label: 'Interest rate (% a year)'
    default: 6.5
    validate:
      min: 0
      step: any
  years:
    type: select
    label: Term
    default: '30'
    options:
      '5': '5 years'
      '10': '10 years'
      '15': '15 years'
      '20': '20 years'
      '25': '25 years'
      '30': '30 years'
  monthly:
    type: pro-calculation
    label: 'Monthly repayment'
    formula: 'IF(rate == 0, amount / (years * 12), amount * (rate / 1200) / (1 - POW(1 + rate / 1200, -(years * 12))))'
    prefix: '$'
    decimals: 2
  total_paid:
    type: pro-calculation
    label: 'Total repaid'
    formula: 'monthly * years * 12'
    prefix: '$'
    decimals: 2
  interest:
    type: pro-calculation
    label: 'Of which interest'
    formula: 'total_paid - amount'
    prefix: '$'
    decimals: 2</code></pre>
</details>
</div>
<div class="demo-calc">
[forms-pro name="tip-splitter" /]
<details class="demo-yaml">
<summary>Show the YAML <code>tip-splitter.yaml</code></summary>
<pre><code class="language-yaml"># user/data/forms-pro/forms/tip-splitter.yaml
title: 'Tip and split'
type: forms-pro
mode: calculator
show_title: true
description: 'Work out the tip and what everyone owes.'
fields:
  bill:
    type: number
    label: 'Bill ($)'
    default: 84.5
    validate:
      min: 0
      step: 0.01
  tip:
    type: pro-slider
    label: Tip
    min: 0
    max: 30
    step: 1
    default: 18
    suffix: '%'
  people:
    type: number
    label: 'Splitting between'
    default: 3
    validate:
      min: 1
      max: 50
  tip_amount:
    type: pro-calculation
    label: 'Tip amount'
    formula: 'bill * tip / 100'
    prefix: '$'
    decimals: 2
  bill_total:
    type: pro-calculation
    label: 'Total with tip'
    formula: 'bill + tip_amount'
    prefix: '$'
    decimals: 2
  per_person:
    type: pro-calculation
    label: 'Each person pays'
    formula: 'IF(people &lt; 1, 0, bill_total / people)'
    prefix: '$'
    decimals: 2</code></pre>
</details>
</div>
</div>

<div class="grid demo-calc-row" style="margin-bottom:2.5rem">
<div class="demo-calc">
[forms-pro name="project-estimator" /]
<details class="demo-yaml">
<summary>Show the YAML <code>project-estimator.yaml</code></summary>
<pre><code class="language-yaml"># user/data/forms-pro/forms/project-estimator.yaml
title: 'Website cost estimator'
type: forms-pro
mode: calculator
show_title: true
description: 'A ballpark price and timeline for a new site. Option keys hold the price per page.'
fields:
  pages:
    type: number
    label: 'Number of pages'
    default: 8
    validate:
      min: 1
      max: 200
  tier:
    type: radio
    label: 'Design'
    default: '750'
    options:
      '400': 'From a template ($400 a page)'
      '750': 'Custom design ($750 a page)'
      '1200': 'Custom with illustrations ($1,200 a page)'
  ecommerce:
    type: checkbox
    label: 'Online shop (+$2,500)'
  multilingual:
    type: checkbox
    label: 'Two or more languages (+$1,200)'
  rush:
    type: checkbox
    label: 'Rush job, 25% faster (+20%)'
  subtotal:
    type: pro-calculation
    label: Subtotal
    formula: 'pages * tier + ecommerce * 2500 + multilingual * 1200'
    prefix: '$'
    decimals: 0
  rush_fee:
    type: pro-calculation
    label: 'Rush fee'
    formula: 'IF(rush, subtotal * 0.2, 0)'
    prefix: '$'
    decimals: 0
  total:
    type: pro-calculation
    label: 'Estimated total'
    formula: 'subtotal + rush_fee'
    prefix: '$'
    decimals: 0
  weeks:
    type: pro-calculation
    label: 'Estimated timeline'
    formula: 'CEIL((pages / 4 + 2 + ecommerce * 2 + multilingual) * IF(rush, 0.75, 1))'
    suffix: ' weeks'
    decimals: 0</code></pre>
</details>
</div>
<div class="demo-calc">
[forms-pro name="unit-converter" /]
<details class="demo-yaml">
<summary>Show the YAML <code>unit-converter.yaml</code></summary>
<pre><code class="language-yaml"># user/data/forms-pro/forms/unit-converter.yaml
title: 'Length and temperature'
type: forms-pro
mode: calculator
show_title: true
description: 'Every unit is an option whose key is its size in metres, so one formula converts any pair.'
fields:
  length:
    type: number
    label: Length
    default: 26.2
    validate:
      step: any
  unit_row:
    type: columns
    fields:
      unit_from_col:
        type: column
        fields:
          from_unit:
            type: select
            label: From
            default: '1609.344'
            options:
              '0.01': Centimetres
              '1': Metres
              '1000': Kilometres
              '0.0254': Inches
              '0.3048': Feet
              '1609.344': Miles
      unit_to_col:
        type: column
        fields:
          to_unit:
            type: select
            label: To
            default: '1000'
            options:
              '0.01': Centimetres
              '1': Metres
              '1000': Kilometres
              '0.0254': Inches
              '0.3048': Feet
              '1609.344': Miles
  converted:
    type: pro-calculation
    label: Result
    formula: 'length * from_unit / to_unit'
    decimals: 3
    formula_display: true
  celsius:
    type: number
    label: 'Temperature (°C)'
    default: 21
    validate:
      step: any
  fahrenheit:
    type: pro-calculation
    label: Fahrenheit
    formula: 'celsius * 9 / 5 + 32'
    suffix: ' °F'
    decimals: 1
  kelvin:
    type: pro-calculation
    label: Kelvin
    formula: 'celsius + 273.15'
    suffix: ' K'
    decimals: 2</code></pre>
</details>
</div>
</div>

<div class="grid demo-calc-row" style="margin-bottom:2.5rem">
<div class="demo-calc">
[forms-pro name="bmi-calculator" /]
<details class="demo-yaml">
<summary>Show the YAML <code>bmi-calculator.yaml</code></summary>
<pre><code class="language-yaml"># user/data/forms-pro/forms/bmi-calculator.yaml
title: 'Body mass index'
type: forms-pro
mode: calculator
show_title: true
description: 'BMI from height and weight, with the healthy weight range for your height. A rough guide only.'
fields:
  bmi_row:
    type: columns
    fields:
      bmi_height_col:
        type: column
        fields:
          height_cm:
            type: number
            label: 'Height (cm)'
            default: 175
            validate:
              min: 50
              max: 250
              step: any
      bmi_weight_col:
        type: column
        fields:
          weight_kg:
            type: number
            label: 'Weight (kg)'
            default: 72
            validate:
              min: 10
              max: 400
              step: any
  bmi:
    type: pro-calculation
    label: 'Your BMI'
    formula: 'weight_kg / POW(height_cm / 100, 2)'
    decimals: 1
    formula_display: true
  healthy_min:
    type: pro-calculation
    label: 'Healthy range starts at'
    formula: '18.5 * POW(height_cm / 100, 2)'
    suffix: ' kg'
    decimals: 1
  healthy_max:
    type: pro-calculation
    label: 'and ends at'
    formula: '24.9 * POW(height_cm / 100, 2)'
    suffix: ' kg'
    decimals: 1</code></pre>
</details>
</div>
<div class="demo-calc demo-calc-empty"></div>
</div>
