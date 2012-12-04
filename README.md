Description
===========

Easily consume JSON web services from your Chef cookbooks.

Requirements
============

Chef >= 10.14.x

Attributes
==========

Usage
=====

This cookbook will define a new `consume_json` method and mix it into
`Chef::Recipe`, thus making it automatically available in recipes in any
cookbook.

You can just invoke it passing a URL:

    data = consume_json('http://example.com/etc_hosts.json')
    # ... process data

If you prefer, you can also use a block:

    consume_json('http://example.com/etc_hosts.json') do |data
      # ... process data
    end

In either case, an HTTP request will be made at compile time. If the request
is successful `data` will be a hash representing the returned document. In
case of failure, an exception will be raised.

How you process the returned data is up to you. A made-up example:

    # assuming the returned document looks like this:
    # {"hosts": {"host-a": "192.168.0.12", "host-b": "192.168.0.20", ...}}
    data = consume_json('http://example.com/etc_hosts.json')
    data['hosts'].each do |name, address|
      template "/tmp/hosts/#{name}.txt" do
        variables :address => address
      end
    end

An external JSON document is a valid replacement for a data bag; for
comparison, the above example would look like this if implemented with data
bags:

    hosts = data_bag('hosts')
    hosts.each do |host|
      template "/tmp/hosts/#{host['name']}.txt" do
        variables :address => host['address']
      end
    end

License and Author
==================

Author:: Andrea Campi (<andrea.campi@zephirworks.com>)

Copyright:: 2012, Opscode, Inc

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
