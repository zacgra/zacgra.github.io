---
title: "Rails CheatSheet"
author: zacgra
layout: post
categories: Code
tags: [ruby, rails]
---

# Migrations

### 1. Generate a Migration

```console
rails generate migration CreateUsers
# or
rails g migration CreateUsers
```

### 2. Create a Table

```rb
create_table :products do |t|
  t.string :name
  t.float :price

  t.timestamps
end
```

Supported column types: `:boolean :date :datetime :float :integer :string :text :time`

### Common Methods for Changing Tables

```rb
add_column :table_name, :column_name, :type, options_hash
remove_column :table_name, :column_name
rename_column :table_name, :old_column_name, :new_column_name
rename_table :old_table_name, :new_table_name
add_index :table_name, %i[column1 column2], options_hash
change_column :table_name, :column_name, :type, options_hash
```

### Additional Table Examples

```rb
# Make the id a UUID
create_table :places, id: :uuid do |t|
  t.string :name, null: false
  t.string :address, null: false
  t.string :city, null: false

  t.timestamps
end
```

```rb
# Use references for a foreign key

# Using decimals, for a decimal value, set a scale (num of digits after period) and precision (total numbers)

create_table :internet_speeds, id: :uuid do |t|
  t.references :place, null: false, foreign_key: true, index: true, type: :uuid
  t.decimal :download_speed, null: false, scale: 2, precision: 10
  t.string :download_units, null: false

  t.timestamps
end
```

<!-- ### Adding Column Modifiers -->

### 3. Create a table by running your migration

Edit a migration by running a new migration to make changes to your table. Don't edit after migration.

```console
rails db:migrate
```

# Models

### Associations

Example of `has_many`, `belongs_to`, and `has_many_through`:

```rb
class Physician < ApplicationRecord
  has_many(
    :appointments,
    class_name: 'Appointment',
    foreign_key: :physician_id,
    primary_key: :id
  )

  has_many :patients, through: :appointments, source: :patient
end

class Appointment < ApplicationRecord
  belongs_to(
    :physician,
    class_name: 'Physician',
    foreign_key: :physician_id,
    primary_key: :id
  )

  belongs_to(
    :patient,
    class_name: 'Patient',
    foreign_key: :patient_id,
    primary_key: :id
  )
end

class Patient < ApplicationRecord
  has_many(
    :appointments
    class_name: 'Appointment',
    foreign_key: :patient_id,
    primary_key: :id
  )

  has_many :physicians, through: :appointments, source: :physician
end
```

Ensure that a `belongs_to` is deleted when its `has_many` is destroyed with `dependent: :destroy`

```rb
class Patient < ApplicationRecord
  has_many(
    :appointments
    class_name: 'Appointment',
    foreign_key: :patient_id,
    primary_key: :id,
    dependent: :destroy
  )
```

## Validation

Validations are defined inside models.
Constraints are defined inside migrations.

Common validations:

- `presence: true`
- `uniqueness: true`

Restrict validation with scope:

```rb
class Holiday < ApplicationRecord
  # no two Holidays with the same name for a single year
  validates :name,
            uniqueness: {
              scope: :year,
              message: "should happen once per year"
            }
end
```

Validate that a value is one of allowed possible values:

```rb
validates :status,
          inclusion: {
            in: "PENDING APPROVED DENIED",
            message: "%<value>% is not a valid status"
          }
```
