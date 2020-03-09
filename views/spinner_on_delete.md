# Add a Submitting message and a spinner to a delete link with UJS

# Intro
Turbolinks and UJS make for a site that feels very fast to your users.

Some actions can take a while on your server or your user might be on a high latency link.

Using a little Stimulus magic we can easily replace the delete link with 'Submitting ...' so the user knows what is happening.

Add in a little spinner graphic/font to complete the effect. 

Make sure to visually test and see how this affects your page in various resolutions.



## Code

in your , eg on an index page that allows you to delete items in a table. Put this code 
** view.html.erb
```erb
<div data-controller="destroyer">
  <%= link_to 'Destroy', model_name, method: :delete, remote: true, 
  data: { confirm: 'Are you sure?', action: 'ajax:beforeSend->destroyer#submit ajax:success->destroyer#removed' } %>
  </div>
```

Make sure to repalce `model_name` with your actual model name.

We are doing 2 callbacks here, one is to set the submit message and on ajax:beforeSend callback that UJS uses and the other is to remove the row on ajax:success when the action is completed successfully. 



**app/javascript/controllers/destroyer_controller.js**
```javascript
import { Controller } from "stimulus"

export default class extends Controller {

  // replace delete text with your submit message and spinner
  submit(event) {
    event.target.innerHTML = "<i class=\"fad fa-spinner fa-spin\"></i> Submitting ..."
  }

  removed(event) {
    event.target.parentElement.parentElement.parentElement.remove()
  }

}
```

## Test

It's easy to add a system test to verify that the waiting text is added.

As this test uses ajax, you will need to specify `Capybara.default_max_wait_time = 5` in your `test/application_system_test_case.rb` to make sure it has enough time to run.

```ruby
  test "destroy shows spinner" do
    visit models_url
    # check text exists
    assert_text @model.title
    page.accept_confirm do
      click_on "Destroy", match: :first
    end
    # check for waiting text
    assert_text "Submitting ..."
    # confirm entry is deleted 
    refute_text @model.title
  end
```

## Further Enhancements

Add a callback `ajax:error` and handle when an error occurs with a new method

Add in the ability to customize the text, so it uses the default or uses something different if you specify it in the action.

