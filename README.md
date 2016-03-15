# Data-Confirm Modal

Uses [Bootstrap's modals](http://twitter.github.io/bootstrap/javascript.html#modals)
in place of the browser's builtin `confirm()` API for links generated through Rails'
helpers with the `:confirm` option.

Any link with the `data-confirm` attribute will trigger a Bootstrap modal.

HTML in the modal supported, and also the ability to have the user input a
certain value, for extra willingness confirmation (inspired by GitHub's
"delete repository" function).

## Installation

Add this line to your application's Gemfile:

    gem 'data-confirm-modal', github: 'neb2000/data-confirm-modal'

if you are stuck on Bootstrap 2.3, use the `bootstrap2` branch:

    gem 'data-confirm-modal', github: 'ifad/data-confirm-modal', branch: 'bootstrap2'

Then execute:

    $ bundle

And then require the Javascript from your `application.js`:

    //= require data-confirm-modal

## Usage

### With Rails

By default, the Gem's Javascript overrides Rails' [data-confirm behaviour][]
for you, with no change required to your code. The modal is applicable to
`<a>`, `<button>` and `<input[submit]>`  elements by default.

Example:

    <%= link_to 'Delete', data: {confirm: 'Are you sure?'} %>

The modal's title will be get from the link's `title` attribute value. The
modal text will be taken from the `data-confirm` value. Multiple paragraphs
are created automatically from two newlines (`\n\n`).

The modal's 'confirm' button text can be customized using the `data-commit`
attribute.

    <%= link_to 'Delete', data: {confirm: 'Are you sure?', commit: 'Sure!'} %>

Add a `data-verify` attribute to your input if you want an extra confirmation
from the user. The modal will contain an extra text input, and the user will be
asked to type the verification value before being allowed to proceed.

    <%= link_to 'Delete', data: {confirm: 'Are you sure?', verify: 'Foo', verify_text: 'Type "Foo" to confirm'} %>

You can set global setting using `dataConfirmModal.setDefaults`, for example:

    dataConfirmModal.setDefaults({
      title: 'Confirm your action',
      commit: 'Continue',
      cancel: 'Cancel'
    });

To restore default settings use `dataConfirmModal.restoreDefaults()`.

[data-confirm-behaviour]: http://api.rubyonrails.org/classes/ActionView/Helpers/UrlHelper.html

### Without Rails, with data attributes

Given an element with `data-confirm` attributes in place, such as

    <a id="foo" href="#" data-confirm="Really do this?" data-commit="Do it" data-cancel="Not really"/>

you can then invoke `.confirmModal()` on it using:

    $('#foo').confirmModal();

that'll display the confirmation modal. If the user confirms, then the `#foo`
link will receive a `click` event.

### Without Rails, without data attributes

Use `dataConfirmModal.confirm()` passing any of the supported options, and pass
an `onConfirm` and `onCancel` callbacks that'll be invoked when the user clicks
the confirm or the cancel buttons.

    dataConfirmModal.confirm({
      title: 'Are you sure?',
      text: 'Really do this?',
      commit: 'Yes do it',
      cancel: 'Not really',
      zIindex: 10099,
      onConfirm: function() { alert('confirmed') },
      onCancel:  function() { alert('cancelled') }
    });

A live jsfiddle example is [available here](http://jsfiddle.net/t0m7ayr3/).

### Modal Options

The default [bootstrap modal options](http://getbootstrap.com/javascript/#modals-options)
can be passed either via JavaScript or through data attributes.

     $('#foo').confirmModal({backdrop: 'static', keyboard: false});

or

     <a href="#" data-confirm="Really?" data-backdrop="static" data-keyboard="false">