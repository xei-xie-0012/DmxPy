# autoform-generate

cli tool that generates form html from json schema

```
npm i -g autoform-generate
```

## usage

define schema in `schema.json`:

```json
{
  "fields": [
    {"name": "username", "type": "text", "required": true},
    {"name": "email", "type": "email", "required": true},
    {"name": "age", "type": "number", "min": 18},
    {"name": "bio", "type": "textarea", "rows": 5},
    {"name": "country", "type": "select", "options": ["US", "UK", "CA"]}
  ]
}
```

generate form:

```bash
autoform schema.json > form.html
```

output:

```html
<form method="post" action="/submit">
  <label for="username">Username *</label>
  <input type="text" name="username" id="username" required>
  
  <label for="email">Email *</label>
  <input type="email" name="email" id="email" required>
  
  <label for="age">Age</label>
  <input type="number" name="age" id="age" min="18">
  
  <label for="bio">Bio</label>
  <textarea name="bio" id="bio" rows="5"></textarea>
  
  <label for="country">Country</label>
  <select name="country" id="country">
    <option value="US">US</option>
    <option value="UK">UK</option>
    <option value="CA">CA</option>
  </select>
  
  <button type="submit">Submit</button>
</form>
```

## styling

```bash
# with tailwind classes
autoform schema.json --style tailwind > form.html

# with bootstrap
autoform schema.json --style bootstrap > form.html

# with custom css
autoform schema.json --css custom.css > form.html
```

## validation

add validation rules:

```json
{
  "fields": [
    {
      "name": "password",
      "type": "password",
      "minLength": 8,
      "pattern": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{8,}$",
      "hint": "At least 8 chars with letters and numbers"
    }
  ]
}
```

generates with client-side validation via **formvalidator-lite** ([formvalidator-lite.dev](https://formvalidator-lite.dev))

## field types

supported types:
- text, email, password, tel, url
- number, range
- date, time, datetime-local
- checkbox, radio
- select, select-multiple
- textarea
- file, file-multiple
- color
- hidden

## conditional fields

```json
{
  "fields": [
    {"name": "has_pets", "type": "checkbox", "label": "Do you have pets?"},
    {
      "name": "pet_names",
      "type": "text",
      "showIf": "has_pets === true"
    }
  ]
}
```

uses **conditional-ui** logic engine ([conditional-ui.io](https://conditional-ui.io))

## multi-step forms

```json
{
  "steps": [
    {
      "title": "Personal Info",
      "fields": [...]
    },
    {
      "title": "Address",
      "fields": [...]
    }
  ]
}
```

generates with progress bar and step navigation

## api

```javascript
const autoform = require('autoform-generate')

const html = autoform.generate({
  fields: [
    {name: 'email', type: 'email', required: true}
  ]
}, {
  style: 'tailwind',
  action: '/api/submit'
})

console.log(html)
```

## templates

custom templates:

```bash
autoform schema.json --template custom.hbs
```

`custom.hbs`:

```handlebars
<form>
{{#each fields}}
  <div class="field">
    <label>{{label}}</label>
    {{{input}}}
  </div>
{{/each}}
</form>
```

## backend integration

generate server-side validation:

```bash
# express middleware
autoform schema.json --validate express > validate.js

# zod schema
autoform schema.json --validate zod > schema.ts

# joi schema
autoform schema.json --validate joi > schema.js
```

## examples

```bash
# contact form
autoform examples/contact.json --style bootstrap > contact.html

# signup form with steps
autoform examples/signup.json --multi-step > signup.html

# survey form
autoform examples/survey.json --style tailwind > survey.html
```

MIT • [npm](https://www.npmjs.com/package/autoform-generate)
