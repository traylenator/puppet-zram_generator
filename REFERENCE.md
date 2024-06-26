# Reference

<!-- DO NOT EDIT: This document was generated by Puppet Strings -->

## Table of Contents

### Classes

#### Public Classes

* [`zram_generator`](#zram_generator): Install and configure zram_generator zram mounts

#### Private Classes

* `zram_generator::config`: Configuration for zram_generator entries
* `zram_generator::install`: Install zram-generator pacakge
* `zram_generator::service`: Class to regenerate swap file units

### Defined types

* [`zram_generator::zram`](#zram_generator--zram): Creates a zram device and optionally mounts it

## Classes

### <a name="zram_generator"></a>`zram_generator`

Install and configure zram_generator zram mounts

* **See also**
  * https://github.com/systemd/zram-generator

#### Parameters

The following parameters are available in the `zram_generator` class:

* [`install_defaults`](#-zram_generator--install_defaults)
* [`manage_defaults_package`](#-zram_generator--manage_defaults_package)
* [`zrams`](#-zram_generator--zrams)

##### <a name="-zram_generator--install_defaults"></a>`install_defaults`

Data type: `Enum['installed', 'absent']`

Controls if the package 'zram-generator-defaults' be installed.

Default value: `'absent'`

##### <a name="-zram_generator--manage_defaults_package"></a>`manage_defaults_package`

Data type: `Boolean`

whether the zram-generator-defaults package should be managed

Default value: `$facts['os']['name'] != 'Archlinux'`

##### <a name="-zram_generator--zrams"></a>`zrams`

Data type: `Stdlib::CreateResources`

Hash of zram_generator::zram instances to expand from hiera.

Default value: `{}`

## Defined types

### <a name="zram_generator--zram"></a>`zram_generator::zram`

Creates a zram device and optionally mounts it

* **See also**
  * zram_generator
    * https://github.com/systemd/zram-generator/blob/main/man/zram-generator.conf.md

#### Examples

##### Create zram device with default `zram_size`

```puppet
zram_generator::zram{'zram0':
  ensure => 'present',
}
```

##### Disable a particular swap

```puppet
zram_generator::zram{'zram1':
  ensure => 'absent',
}
```

##### Create zram device with half ram size and host memory limit

```puppet
zram_generator::zram{'zram1':
  host_memory_limit => 9048,
  zram_size         => 'ram / 2',
}
```

##### Create zram device with exact 1024 MiB size

```puppet
zram_generator::zram{'zram2':
  zram_size         => 1024,
}
```

##### Create a ext4 filesystem and mount at location.

```puppet
zram_generator::zram{'zram2':
  fs_type            => 'ext4',
  mount_point        => '/run/compressed-mount-point',
}
```

#### Parameters

The following parameters are available in the `zram_generator::zram` defined type:

* [`device`](#-zram_generator--zram--device)
* [`ensure`](#-zram_generator--zram--ensure)
* [`host_memory_limit`](#-zram_generator--zram--host_memory_limit)
* [`zram_size`](#-zram_generator--zram--zram_size)
* [`compression_algorithm`](#-zram_generator--zram--compression_algorithm)
* [`writeback_device`](#-zram_generator--zram--writeback_device)
* [`swap_priority`](#-zram_generator--zram--swap_priority)
* [`mount_point`](#-zram_generator--zram--mount_point)
* [`fs_type`](#-zram_generator--zram--fs_type)
* [`options`](#-zram_generator--zram--options)

##### <a name="-zram_generator--zram--device"></a>`device`

Data type: `Pattern[/\Azram\d+\z/]`

The name of device to create, e.g. `zram0`.

Default value: `$title`

##### <a name="-zram_generator--zram--ensure"></a>`ensure`

Data type: `Enum['present','absent']`

Configure and start device or stop and de-configure device

Default value: `'present'`

##### <a name="-zram_generator--zram--host_memory_limit"></a>`host_memory_limit`

Data type: `Variant[Enum['none'],Integer[0]]`

The maximum amount of memory (in MiB). If set to `none` then there will be no limit.

Default value: `'none'`

##### <a name="-zram_generator--zram--zram_size"></a>`zram_size`

Data type: `Variant[Integer[1], String[1]]`

The size of the zram device, as a function of MemTotal or absolute, both in MB.

Default value: `'min(ram / 2, 4096)'`

##### <a name="-zram_generator--zram--compression_algorithm"></a>`compression_algorithm`

Data type: `Optional[String[1]]`

Specifies the algorithm used to compress the zram device. If unset will use kernel's default.

Default value: `undef`

##### <a name="-zram_generator--zram--writeback_device"></a>`writeback_device`

Data type: `Optional[Stdlib::Unixpath]`

Write incompressible pages, for which no gain was achieved, to the specified device under memory pressure.
This corresponds to the `/sys/block/zramX/backing_dev` parameter.
Takes a path to a block device, like `/dev/disk/by-partuuid/2d54ffa0-01` or `/dev/zvol/tarta-zoot/swap-writeback`.
If unset, none is used, and incompressible pages are kept in RAM.

Default value: `undef`

##### <a name="-zram_generator--zram--swap_priority"></a>`swap_priority`

Data type: `Integer[-1,32767]`

Controls the relative swap priority. Higher numbers indicate higher priority.

Default value: `100`

##### <a name="-zram_generator--zram--mount_point"></a>`mount_point`

Data type: `Optional[Stdlib::Unixpath]`

Format the device with a file system (not as swap) and mount this file system over
the specified directory. When neither this option nor `fs_type` is specified,
the device will be formatted as swap.

Default value: `undef`

##### <a name="-zram_generator--zram--fs_type"></a>`fs_type`

Data type: `Optional[String[1]]`

Specifies how the device shall be formatted. The default is ext2 if mount-point is specified, and swap
otherwise. (Effectively, the device will be formatted as swap, if neither `fs_type` nor `mount_point` are specified.)
Note that the device is temporary: contents will be destroyed automatically after the file system is unmounted
(to release the backing memory).

Default value: `undef`

##### <a name="-zram_generator--zram--options"></a>`options`

Data type: `String[1]`

Sets mount or swapon options. Availability depends on fs-type.

Default value: `'discard'`

