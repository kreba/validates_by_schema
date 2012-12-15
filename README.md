# Validates By Schema (validates_by_schema)
[![Build Status](https://secure.travis-ci.org/joshwlewis/validates_by_schema.png)](http://travis-ci.org/joshwlewis/validates_by_schema) 

Automatic validation based on your database schema column types and limits. Keep your code DRY by inferring column validations from table properties!

## Install

1. Add it to your Gemfile:

```ruby
gem "validates_by_schema"
```

2. Then `bundle`

3. Call it from your ActiveRecord model:

```ruby
class Widget < ActiveRecord::Base
  validates_by_schema
end
```

## Usage

You can also whitelist/blacklist columns with :only and :except options

```ruby
validates_by_schema only: [:body, :description]
```

```ruby
validates_by_schema except: [:name, :title]
```

## Example

Say you had a table setup like this:
```ruby
create_table "widgets", :force => true do |t|
  t.integer  "quantity", :limit => 2
  t.decimal  "thickness", :precision => 4, :scale => 4
  t.string   "color", :null => false
end
```

Then these validations are inferred when you add `validates_by_schema` to your model:

```ruby
validates :wheel_count, numericality: { allow_nil: true,
  greater_than: -32768, less_than: 32768}
validates :thickness, numericality: {allow_nil: true, 
  less_than_or_equal_to: 0.999, greater_than_or_equal_to: -0.999}
validates :color, presence: true, length: {allow_nil: true, maximum: 255}
```

This is tested with postgres, mysql, and sqlite3. It will most likely work with any other database Rails supports.