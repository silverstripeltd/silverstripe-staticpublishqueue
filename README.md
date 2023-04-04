# Static Publisher with Queue

[![CI](https://github.com/silverstripe/silverstripe-staticpublishqueue/actions/workflows/ci.yml/badge.svg)](https://github.com/silverstripe/silverstripe-staticpublishqueue/actions/workflows/ci.yml)
[![Silverstripe supported module](https://img.shields.io/badge/silverstripe-supported-0071C4.svg)](https://www.silverstripe.org/software/addons/silverstripe-commercially-supported-module-list/)

## Brief

This module provides an API for your project to be able to generate a static cache of your pages to enhance
performance by not booting Silverstripe in order to serve requests.

It generates the cache files using the [QueuedJobs module](https://github.com/symbiote/silverstripe-queuedjobs).

[Docs](docs/en/index.md)

## Requirements

* "silverstripe/framework": "^5",
* "silverstripe/cms": "^5",
* "silverstripe/config": "^2",
* "symbiote/silverstripe-queuedjobs": "^5",
* "silverstripe/versioned": "^2"

## Migrating from v4

### `SiteTreePublishingEngine` changes

The following methods have been removed:

* `getToUpdate()`
* `setToUpdate()`
* `getToDelete()`
* `setToDelete()`

These methods used to deal with `DataObjects`. We are now collecting URLs instead of `DataObjects`, and these URLs are
being stored and manipulated by the following methods.

* `getUrlsToUpdate()`
* `addUrlsToUpdate()`
* `setUrlsToUpdate()`
* `getUrlsToDelete()`
* `addUrlsToDelete()`
* `setUrlsToDelete()`

Why did this change happen? TL;DR: We realised that purging URLs for pages that change their URL (EG: their parent page
changes, or their `URLSegment` changes) was not functioning as expected. This was really the only way to enable this
functionality to work correctly while maintaining cache integrety.

## Unit-testing with StaticPublisherState to disable queuedjobs for unit-tests

You can use `StaticPublisherState` to disable queuejobs job queueing and logging in unit-testing to improve performance.

Add the following yml to your project:

```yml
----
Name: staticpublishqueue-tests
Only:
  classexists:
    - 'Symbiote\QueuedJobs\Tests\QueuedJobsTest\QueuedJobsTest_Handler'
    - 'SilverStripe\StaticPublishQueue\Test\QueuedJobsTestService'
----
SilverStripe\Core\Injector\Injector:
  SilverStripe\Dev\State\SapphireTestState:
    properties:
      States:
        staticPublisherState: '%$SilverStripe\StaticPublishQueue\Dev\StaticPublisherState'
```
