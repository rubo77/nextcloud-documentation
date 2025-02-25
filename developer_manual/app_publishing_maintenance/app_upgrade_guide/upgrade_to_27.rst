=======================
Upgrade to Nextcloud 27
=======================

.. note:: Critical changes were collected `on GitHub <https://github.com/nextcloud/server/issues/37039>`_.
    See the original ticket for links to the pull requests and tickets.

General
-------

info.xml
^^^^^^^^

Make sure your ``appinfo/info.xml`` allows for Nextcloud 27.

.. code-block:: xml

    <dependencies>
        <nextcloud min-version="25" max-version="27" />
    </dependencies>

Front-end changes
-----------------

Added APIs
^^^^^^^^^^

* A new Files Router API allows you to control the files router service and update views, querys or param without page reload. See :ref:`FilesAPI`

Back-end changes
----------------

Removal of PSR-0 class loader
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Nextcloud 27 no longer loads classes in the deprecated :ref:`PSR-0 naming standard<psr0>`. :ref:`Update the structure to PSR-4<app-psr4-autoloader>` or :ref:`ship a custom autoloader<app-custom-classloader>`.

Added APIs
^^^^^^^^^^

* ``\OCP\AppFramework\Utility\ITimeFactory`` is now a ``\PSR\Clock\ClockInterface`` following the `PSR-20 standard <https://www.php-fig.org/psr/psr-20/#21-clockinterface>`_. (`nextcloud/server#35872 <https://github.com/nextcloud/server/pull/35872>`_)
* ``\OCP\Accounts\IAccountManager::PROPERTY_DISPLAYNAME_LEGACY`` was added to keep the provisioning API compatible. (`nextcloud/server#36665 <https://github.com/nextcloud/server/pull/36665>`_)
* A new system config ``ratelimit.protection.enabled`` (boolean, default true) was added to allow developers to **disable** rate limiting. It is **not** recommended to disable this on a production/live system. (`nextcloud/server#37542 <https://github.com/nextcloud/server/pull/37542>`_)
* A new OCP API under ``\OCP\SpeechToText`` was introduced to allow automatic transcription of media (`nextcloud/server#37674 <https://github.com/nextcloud/server/pull/37674>`_)
* Added a new interface ``\OCP\BackgroundJob\IParallelAwareJob`` that ``\OCP\BackgroundJob\Job`` now implements. It can be used to allow specifying that multiple instances of a job should not be run at the same time. Also added ``\OCP\BackgroundJob\IJobList#hasReservedJob(?string $className = null)`` method to check the condition (`nextcloud/server#37835 <https://github.com/nextcloud/server/pull/37835>`_)
* New property ``$actionLabel`` has been added to the ``\OCP\Files\Template\TemplateFileCreator`` class with a respective setter ``TemplateFileCreator::setActionLabel`` and getter ``TemplateFileCreator::getActionLabel``.  (`nextcloud/server#37929 <https://github.com/nextcloud/server/pull/37929>`_ + `nextcloud/server#37955 <https://github.com/nextcloud/server/pull/37955>`_)
* A new interface ``\OCP\Group\Backend\ISearchableGroupBackend`` was added for group backends supporting new method ``searchInGroup`` to search among a group users in an efficient way. (`nextcloud/server#32866 <https://github.com/nextcloud/server/pull/32866>`_)


Deprecated APIs
^^^^^^^^^^^^^^^

* ``\OCP\AppFramework\Utility\ITimeFactory::getTime()`` and ``\OCP\AppFramework\Utility\ITimeFactory::getDateTime()`` were deprecated, because the interface is now a ``\PSR\Clock\ClockInterface`` following the `PSR-20standard <https://www.php-fig.org/psr/psr-20/#21-clockinterface>`_. (`nextcloud/server#35872 <https://github.com/nextcloud/server/pull/35872>`_)
* ``\OCP\GroupInterface::usersInGroup()`` is deprecated in favor of newly added ``\OCP\Group\Backend\ISearchableGroupBackend`` interface. (`nextcloud/server#32866 <https://github.com/nextcloud/server/pull/32866>`_)

Removed APIs
^^^^^^^^^^^^

* Intermediate transition event classes ``\OCP\WorkflowEngine\IEntityCompat`` and ``\OCP\WorkflowEngine\IOperationCompat`` have been removed as advertised for 2023 (`nextcloud/server#37040 <https://github.com/nextcloud/server/pull/37040>`_)


Behavioral changes
^^^^^^^^^^^^^^^^^^

* ``\OCP\Files\Cache\CacheEntryRemovedEvent`` will now be dispatched for all files and folders inside the deleted node. (`nextcloud/server#34773 <https://github.com/nextcloud/server/pull/34773>`_)

Client APIs
-----------

Changed APIs
^^^^^^^^^^^^

* The OCS translation API was extended to return the ``from`` language attribute so in case no from was given, clients can afterwards show in the UI which language was detected and used for translating. (`nextcloud/server#38003 <https://github.com/nextcloud/server/pull/38003>`_)
