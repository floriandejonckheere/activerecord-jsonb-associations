# Activerecord::Jsonb::Associations

Use PostgreSQL JSONB fields to store association information of your models.

**Requirements:**

- PostgreSQL (>= 9.6)

## Usage

### One-to-one and One-to-many associations

```ruby
class Profile < ActiveRecord::Base
  # Setting additional :store option on :belongs_to association
  # enables saving of foreign ids in :extra JSONB column 
  belongs_to :user, store: :extra
end

class SocialProfile < ActiveRecord::Base
  belongs_to :user, store: :extra
end

class User < ActiveRecord::Base
  # Parent model association needs to specify :foreign_store
  # for associations with JSONB storage
  has_one :profile, foreign_store: :extra
  has_many :social_profiles, foreign_store: :extra
end
```

Foreign keys for association on one model have to be unique, even if they use different store column.

You can also use `add_references` in your migration to add JSONB column and index for it (if `index: true` option is set):

```ruby
add_reference :profiles, :users, store: :extra, index: true
```

### Many-to-many associations

```ruby
class Label < ActiveRecord::Base
  # extra['user_ids'] will store associated user ids
  has_and_belongs_to_many :users, store: :extra
end

class User < ActiveRecord::Base
  # extra['label_ids'] will store associated label ids
  has_and_belongs_to_many :labels, store: :extra
end
```

TODO: add benchmarks for regular vs. jsonb HABTM associations.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'activerecord-jsonb-associations'
```

And then execute:

```bash
$ bundle install
```

## Developing

To setup development environment, just run:

```bash
$ bin/setup
```

## License
The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
