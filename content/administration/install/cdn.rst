===========================================
Setting up a Content Delivery Network (CDN)
===========================================

.. _reference/cdn/keycdn:

Deploying with KeyCDN
=====================

.. sectionauthor:: Fabien Meghazi / Timothy Kukulka

A :abbr:`CDN (Content Delivery Network)` or Content Distribution Network, is a geographically
distributed network of servers that provides high speed internet content. The
:abbr:`CDN (Content Delivery Network)` provides quick, quality content for content heavy websites.

This document will guide you through the setup of a KeyCDN_ account with an Odoo powered website.

Step 1: Create a pull zone in the KeyCDN dashboard
--------------------------------------------------

On the KeyCDN dashboard, navigate to :guilabel:`Zones` menu item on the left. Name the
:guilabel:`Zone Name`. This name will appear in the :abbr:`URL (Uniform Resource Locator)`. The
:guilabel:`Zone Status` can be toggled back and forth from :guilabel:`Active` to
:guilabel:`Inactive`. For this process the :guilabel:`Zone Type` should be :guilabel:`Pull`. Under
:guilabel:`Pull Settings` enter the :guilabel:`Origin URL`. This should be the full database
:abbr:`URL (Uniform Resource Locator)`.

.. example::
   Use ``https://yourdatabase.odoo.com``. Replace *yourdatabase* with the name of the database or the
   custom URL.

.. image:: cdn/keycdn-zone.png
   :align: center
   :alt: KeyCDN's Zone configuration page.

Under :guilabel:`General Settings` click the :guilabel:`Show all settings` button to expand the
Zone options. This should be the last option on the page. After expanding the
:guilabel:`General Settings` ensure that the :guilabel:`CORS` option is enabled. By default this
option should be *enabled*.

Next, scroll to the bottom of the Zone configuration page and :guilabel:`Save` the changes. KeyCDN
will indicate that the new Zone will be deployed, this can take about 10 minutes.

.. image:: cdn/zone-url.png
   :align: center
   :alt: KeyCDN deploying the new Zone.

.. note::
   A new :guilabel:`Zone URL` has been generated for your Zone, in this example it is
   ``pulltest-xxxxx.kxcdn.com``. This will differ for each database.

Copy this :guilabel:`Zone URL` to a Notepad as it will be used in the next steps.

Step 2: Configure the Odoo instance with the new zone
-----------------------------------------------------

In the Odoo Website App, go to the :menuselection:`Settings` and then activate the
:guilabel:`Content Delivery Network (CDN)` and copy/paste the :guilabel:`zone URL` in the
:guilabel:`CDN Base URL` field. This field is only visible and configurable when
:doc:`../../applications/general/developer_mode` is activated.

.. note::
   Ensure that there are two "//"" *Forward Slashes* before the :guilabel:`CDN Base URL` and one "/"
   *Forward Slash* after the :guilabel:`CDN Base URL`.

:guilabel:`Save` the Settings.

.. image:: cdn/cdn-base-url.png
   :align: center
   :alt: Activate the CDN setting in Odoo.

Now the website is using the CDN for the resources matching the :guilabel:`CDN filters` regular
expressions.

In the HTML of the Odoo website the CDN integration is evidenced as working properly by checking the
:abbr:`URL (Uniform Resource Locators)` of images. The CDN Base URL can be seen using the inspect
function on the Odoo website.

.. image:: cdn/test-pull.png
   :align: center
   :alt: The CDN Base URL can be seen using the inspect function on the Odoo website.

Why activate CORS?
------------------

A security restriction in some browsers (Firefox and Chrome) prevents a remotely linked CSS file to
fetch relative resources on this same external server.

If the CORS option isn't enabled in the :guilabel:`CDN zone`, the more obvious resulting problem on
a default Odoo website will be the lack of font-awesome icons because the font file declared in the
font-awesome CSS won't be loaded on the remote server.

A security error message will appear in the browser's console:

.. image:: cdn/odoo-security-message.png
   :align: center
   :alt: Error message populated in the browser console.

Enabling the CORS option in the :abbr:`CDN (Content Delivery Network)` fixes this issue.

.. _KeyCDN: https://www.keycdn.com
