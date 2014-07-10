# Tsv
[![Build Status](https://travis-ci.org/mimimi/ruby-tsv.svg?branch=master)](https://travis-ci.org/mimimi/ruby-tsv)

A simple TSV parser, developed with aim of parsing a ~200Gb TSV dump. As such, no mode of operation, but enumerable is considered sane. Feel free to use `#to_a` on your supercomputer :)

Does not (yet) provide TSV writing mechanism. Pull requests are welcome :)

## Installation

Add this line to your application's Gemfile:

    gem 'tsv'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install tsv

## Usage

### High level interfaces

#### TSV::parse

`TSV.parse` accepts TSV as a whole string, returning lazy enumerator, yielding TSV::Row objects on demand

#### TSV::parse_file

`TSV.parse_file` accepts path to TSV file, returning lazy enumerator, yielding TSV::Row objects on demand
`TSV.parse_file` is also aliased as `[]`, allowing for `TSV[filename]` syntax

#### TSV::Row

By default TSV::Row behaves like an Array of strings, derived from TSV row. However this similarity is limited to Enumerable methods. In case a real array is needed, `#to_a` will behave as expected.
Additionally TSV::Row contains header data, accessible via `#header` reader.

In case a hash-like behaviour is required, field can be accessed with header string key. Alternatively, `#with_header` and `#to_h` will return hash representation for the row.

### Examples

Getting first line from tsv file without headers:
```ruby
TSV.parse_file("tsv.tsv").without_header.first
```

Mapping name fields from a file:
```ruby
TSV["tsv.tsv"].map do |row|
  row['name']
end
```

Mapping last and first row elements:
```ruby
TSV["tsv.tsv"].map do |row|
  [row[-1], row[1]]
end
```

### Nuances

Range accessor is not implemented for initial version due to authors' lack of need.
In addition, accessing tenth element in a row of five is considered an exception from TSV standpoint, which should be represented in range accessor. Such nuance, would it be implemented, will break expectations. Still, if need arises, pull or feature requests with accompanying reasoning (or even without one) are more than welcome.

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
