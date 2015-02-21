---
layout:     post
title:      "Testing Rails with Rspec - Cheat Sheet"
description:  "Some basic notes about testing with RSpec. Taken from the relevant CodeSchool class."
date:       2014-11-13 12:00:00
tags:
  - rails
  - rspec
  - testing
  - tdd/bdd
author:     George Banis
---
> This cheat sheet was influenced by [Code School's Testing with Rspec](http://rspec.codeschool.com/) course. I highly recommend you try the course!

## Installing

### Add the gem

```ruby
group :development, :test do
  gem 'rspec-rails'
end
```

### Initialize

For a rails project:

`$ rails generate rspec:install`

For a simple ruby project

`$ rspec --init`

## Running Rspec

Running all specs:

`$ rspec`

Running all specs in a specific directory:

`$ rspec spec/models`

Running a specific file:

`$ rspec spec/models/zombie_spec.rb`

To run an example on a specific line, specify line number:

`$ rspec spec/models/zombie_spec.rb:4`

### Formatted results

Colorful test results:

`$ rspec --color spec/models/zombie_spec.rb`

Verbose output

`$ rspec --color --format documentation spec/models/zombie_spec.rb`

You can also put these commands in the .rspec file
```
--color
--format documentation
```

## A Basic Spec File

To **describe** the spec, in the `spec` file:

```ruby
# /spec/lib/zombie_spec.rb

require "spec_helper"
require "zombie"  # Require the class

describe Zombie do  # Zombie is the name of the class we test
  it "is named Ash" do
    zombie = Zombie.new
    zombie.name.should == 'Ash' # expectation
  end

  it "has no brains" do
    zombie = Zombie.new
    zombie.brains.should < 1 # ('should' = matcher, '<' = modifier)
  end

end

# lib/zombie.rb
class Zombie
  attr_accessor :name :brains

  def initialize
    @name = 'Ash'
    @brains = 0
  end
end
```

## Matchers

```ruby
zombie.name.should == 'Ash'

zombie.alive.should == false
zombie.alive.should be_false

zombie.rotting.should == true
zombie.rotting.should be_true

zombie.height.should > 5
zombie.brains.should be > 5

zombie.hungry?.should be_true
zombie.should be_hungry   # this runs the 'hungry?' method

zombie.should be_valid

zombie.name should match(/Ash Clone \d/)  # to match a regexp

zombie.tweets.should include(tweet1)  # tests if item (tweet1) is part of an array (tweets)

zombie.weapons.count.should == 2
zombie.should have(2).weapons  # same as above
zombie
zombie.should have_at_least(2).weapons
zombie.should have_at_most(2).weapons

expect { zombie.save }.to change { Zombie.count }.by(1)  # Zombie.count runs before and after expect, results are compared with number in 'by'
expect { zombie.save }.to change { Zombie.count }.from(1).to(2)

expect { zombie.save! }.to raise_error (ActiveRecord:RecordInvalid)  # passing an exception is optional
expect { zombie.save! }.not_to raise_error
expect { zombie.save! }.to_not raise_error  # same as above

# respond_to(<method_name>)
@zombie.should respond_to(hungry?)

# be_within(<range>).of(<expected>)
@width.should be_within(0.1).of(33.3)

# exist
@zombie.should exist

# satisfy { <block> }
@zombie.should satisfy { |zombie| zombie.hungry? }

# be_kind_of(<class>)
@hungry_zombie.should be_kind_of(Zombie)  # HungryZombie < Zombie

# be_an_instance_of(<class>)
@status.should be_an_instance_of(String)
```

## Mark as pending

```ruby
xit "is named Ash" do
  ...
end

# or

it "is named Ash" do
  pending
  ...
end
```

## DRY SPECS

```ruby
# Shortening code
describe Zombie do
  it 'responds to name' do
    subject.should respond_to(:name)  # 'subject' = Zombie.new
  end
end

describe Zombie do
  it 'responds to name' do
    should respond_to(:name)  # we don't need to specify subject
  end
end

describe Zombie do
  it { should respond_to(:name) }  # even shorter
  end
end
```

```ruby
# Best code
describe Zombie do
  its(:name) { should == 'Ash' }  # even shorterer
  its(:weapons) { should include(weapon) }
  its(:brains) { should be_nil }
  its('tweets.size') { should == 2 }
end

```

### Nesting

```ruby
describe Zombie do
  describe 'when hungry' do
    it 'craves brains'
    describe 'with a veggie preference' do
      it 'still craves brains'
      it 'prefers vegan brains'
    end
  end
end

# the same thing can be written using 'context'
describe Zombie do
  context 'when hungry' do
    it 'craves brains'
    context 'with a veggie preference' do
      it 'still craves brains'
      it 'prefers vegan brains'
    end
  end
end
```

### Using Subject

```ruby
...
  context 'with a veggie preference' do
    subject { Zombie.new(vegetarian: true) }

    it 'craves vegan brains' do
      craving.should == 'vegan brains'
      its(:craving) { should == 'vegan brains' }
    end
  end
```

Newer syntax:

```ruby
context 'with a veggie preference' do
  subject(:zombie) { Zombie.new(vegetarian: true, weapons: [axe])}
  let(:axe) { Weapon.new(name: 'axe')  }
```

## Hooks

To run a piece of code before each test:

```ruby
  before { zombie.hungry! }
  before(:each) {}  # Same as above
  before(:all) {}
  after(:each) {}
  after(:all) {}
```

## Shared Examples

```ruby
# spec/models/zombie_spec.rb
describe Zombie do
  it_behaves_like 'the undead'
end

# spec/models/vampire_spec.rb
describe Vampire do
  it_behaves_like 'the undead'
end

# Shared Example
# spec/support/shared_examples_for_undead.rb
shared_examples_for 'the undead' do
  it 'does not have a pulse' do
    subject.pulse.should == false  # the implicit 'subject'
  end
end
```

Same as above:

```ruby
# spec/models/zombie_spec.rb
describe Zombie do
  it_behaves_like 'the undead' do
    let(:undead) { Zombie.new }
  end
end

# spec/support/shared_examples_for_undead.rb
shared_examples_for 'the undead'
  it 'does not have a pulse' do
    undead.pulse.should == false  # we can access the 'undead' defined in 'let'
  end
end
```

Same as above:

```ruby
# spec/models/zombie_spec.rb
describe Zombie do, Zombie.new  # pass the instance as the second parameter
  it_behaves_like 'the undead'
end

# spec/support/shared_examples_for_undead.rb
shared_examples_for 'the undead' do |undead|  # pick the second parameter (Zombie instance) as 'undead'
  it 'does not have a pulse' do
    undead.pulse.should == false
  end
end
```

## Metadata and Filters

```ruby
# spec/lib/zombie_spec.rb
describe Zombie do
  context 'when hungry' do
    it 'wants brains'
    context 'with a veggie preference', slow:true do
      it 'still craves brains'
      it 'prefers vegan brains'
    end
  end
end
```

Runs all tests filtered with slow:

`$ rspec --tag slow spec/lib/zombie_spec.rb`

Runs tests NOT filtered with slow:

`$ rspec --tag ~slow spec/lib/zombie_spec.rb`

## Mocking and Stubbing

`Stub` replaces a method with code that returns a specified result.

`Mock` is a stub with expectations that the method gets called.

```ruby
zombie.weapon.stub(:slice)  # blocks the method slice
zombie.weapon.should_receive(:slice)
```
