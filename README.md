# Transdifflation [![travis-ci](https://secure.travis-ci.org/Sage/transdifflation.png)](http://travis-ci.org/#!/Sage/transdifflation)

What is Transdifflation? Transdifflation is a portmanteau of 'Translation' and 'Diff'.  It helps you to manage the translation of Rails i18n strings that appear in your application and the Ruby Gems it includes.

It compares two .yml locate files, one in your source code (target) and one in others' (gems, other projects) source code (source) and generates a beside-file with the keys you haven't translated yet.
Then you can merge it. It is designed to detect changes between versions. For now, the target file cannot be renamed, and it is generated in 'config/locales/' + to_locale (param in task). Names are inferred by the task params.

It never changes your source files (unless they don't yet exist, in which case they are created for you). 

## Installation

Add this line to your application's Gemfile:

    gem 'transdifflation'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install transdifflation

## Usage

It needs a config file (**transdifflation.yml**) placed on your host root, or in 'config'. If not exists, a rake task is set up to generate it: ```rake transdifflation:config```. Config file looks like:

```yml
tasks:
  task_name1: {
    desc: 'Get diff translation from file config/locales/en.yml gem gem_name',
    type: gem,
    params: {
      gem_name: 'gem_name',
      path_in_gem: 'config/locales/en.yml',
      from_locale: 'en',
      to_locale: 'es'
    }
  }
  task_name2: {
    desc: 'Get diff translation from file config/locales/en.yml file in file_path',
    type: file,
    params: {
      tag_name: 'tag_for_file',
      file_path_from_rails_root: '../another_project/config/locales/en.yml',
      from_locale: 'en',
      to_locale: 'fr'
    }
  }
grouped_tasks:
  task_group_name: 
    - task_name1
    - task_name2
```

These nodes generates rake tasks. There are two types of tasks:

*   type **gem**: When it rans, it checks where the gem 'gem_name' is installed, and looks inside the gem for the file located in 'path_in_gem'. It uses from_locale and to_locale to translate names and keys inside yaml.

*   type **file**: When it rans, it looks for the file in 'file_path_from_rails_root' is installed. It uses from_locale and to_locale to translate names and keys inside yaml. Tag_name is used to name target file in our host.

Also, you can create grouped tasks in a node called 'grouped_taks'. Task **all** is automatially generated.  

Execute ```rake -T``` to determine sucess of config file. Your tasks should appear there, under namespace ```transdifflation:``




## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request