[Contributing to This Project](CONTRIBUTING.md)

ActiveConfig
============

The ActiveConfig module is a platform that allows other modules to
dynamically inject configuration fields into the system configuration
of a third module. The ActiveConfig module activates after Magento has loaded all of the system.xml files. It looks through the loaded config for any sections that have an import specification. It then fires an event for the module whose config is to be imported to handle.

## Config Path

The config path is required to retrieve configuration that was saved
from fields generated using the ActiveConfig module.

The config path is a string that becomes the base of the path string.
It is of the following format:

	module_config_section_name/module_config_group_name

For example the config path 'testsection/testgroup' matches the following config in the system.xml:
	<config>
		<sections>
			<testsection>
				<groups>
					<testgroup>
						<fields>
							<activeconfig_import>
								…

## Import Specification

The import specification is expected to have the following structure:

	<config>
		<sections>
			…
			<groups>
				…
				<fields>
					<activeconfig_import> <!-- signals that an import is necessary -->
						<module>            <!-- module whose feature config we want to add -->
							…               <!-- magento settings (sort_order, show_in_*) -->
						<feature/>        <!-- feature whose config we're importing -->
						<feature2>
							…               <!-- magento settings (sort_order, show_in_*) -->
						</feature2>
						</module>
					</activeconfig_import>
				</fields>
			</groups>
		</sections>

The order of the the import specification within the fields node doesn't matter.

So the following two examples are equivalent.

	<fields>
		<some_field>…</some_field>
		<activeconfig_import/>…</activeconfig_import>
	</fields>

	<fields>
		<activeconfig_import>…</activeconfig_import>
		<some_field>…</some_field>
	</fields>

The following settings are supported at the module level. That is all fields generated by the module will use these settings when generating the fields.

 - sort_order
 - show_in_default
 - show_in_website
 - show_in_store

The values are basically copied straight into the field's xml structure. The sort_order setting is will be the sort_order value of the first field generated and is incremented by one for each additional field.

## Events

The ActiveConfig module listens for the the
`adminhtml_init_system_config` event.

The events that ActiveConfig fire are taken straight from
the import spec.

in the above configuration, the event fired would
be `activeconfig_import_module`. As such, modules that support importing config define an event handler for its respective event.

License and Copyright
=====================

Copyright © 2014 eBay Enterprise

Licensed under the terms of the Open Software License v. 3.0 (OSL-3.0). See [LICENSE.md](LICENSE.md) or http://opensource.org/licenses/OSL-3.0 for the full text of the license.

