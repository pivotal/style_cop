# style_cop
Enforce markup structure for front-end patterns via Rspec and Capybara. You spent all that time making your CSS play nicely. If all developers use the markup correctly, then it will render beautifully! 

Other benefits:

* Creates a momentum to "use or improve" front-end CSS patterns, rather than page-overriding to tweak
* Test-Drive creating your markup! Use the pattern, assert on it and fix the reds!

## Installation

Put into your gemfile

    gem style_cop
    
Bundle

    $ bundle
    
## Getting Started
Find a font-end pattern you want to enforce. 

For example, here is an example piece of markup (a feedback widget with two avatars):

	<article class="feedback">
      <h2 class="feedback-title"></h2>
	  <div class="content-section">
	    <section class="avatars">
	   	  <div class="avatar recipient">
	   	  	<img src="http://placekitten.com/100/100" alt="A cute burmese kitten!" />
		  </div>
		  <div class="avatar provider">
	   	  	<img src="http://placekitten.com/50/50" alt="An abyssinian kitty!" />		  </div>
	    </section>
	  </div>
	</article>

To enforce the markup, you need Capybara installed. Then in a feature test, register your rules:

	register_style_structure_for('.feedback') do
      has_child '.feedback-title'
      has_nested_children '.content-section', '.avatars', '.avatar.recipient'
      has_nested_children '.content-section', '.avatars', '.avatar.provider'
    end
 
Once your rules are registered, you can then enforce the rules on ANY page Capybara looks at with:

	assert_style_structure_for('.feedback')
	
Your tests now will break if the markup doesn't follow the registered structure!
	
## Example
Here is an example file setup:

feedback_style_spec.rb

	register_style_structure_for('.feedback') do
      has_child '.anchor'
      has_nested_children '.content-section', '.avatars', '.avatar.recipient'
      has_nested_children '.content-section', '.avatars', '.avatar.provider'
    end

	feature 'Feedback' do
	  scenario "testing css class structure" do
	    When "I visit my feedback page"
	    Then "I see the feedback avatar pattern"
	  end
		
	  def i_visit_my_feedback_page
	    visit feedback_path
	  end
	  
	  def i_see_the_feedback_avatar_pattern
	  	assert_style_structure_for('.feedback')
	  end
	end