#####################################
ارتقاء, بازگردانی و حذف نصب
#####################################

.. contents::
	:local:
	:depth: 2

.. _upgrade_bos:

****************
ارتقاء فریم‌ور
****************

روند بروز رسانی فریم‌ور از یک‌مکانیسم استاندارد برای نصب / بروز رسانی بسته های نرم افزاری داخل هر سیستمِ بر پایهء OpenWrt استفاده میکند. مراحل زیر را برای بروز رسانی فریم‌ور  دنبال کنید.

ارتقاء از طریق رابط کاربری وب
==============================

فریم‌ور به طور مرتب وجود نسخه جدید را چک و به طور خودکار سیستم را به روز رسانی میکند. در صورتی که گزینه بروز رسانی خودکار غیر فعال باشد، یک دکمه آبی رنگ با عنوان **Upgrade** در سمت راست نوار بالایی نمایان میشود. روی آن کلیک کنید و شروع ارتقا را تایید کنید.

به عنوان روش جایگزین، میتوانید اطلاعات مخزن را به صورت دستی با کلیک بر روی دکمه **Update lists** در بخش منوی System > Software بروز رسانی کنید. در صورت عدم وجود دکمه، صفحه را رفرش کنید. برای شروع پروسه ارتقا، کلمه ``firmware`` را داخل پکیج دانلود و نصب تایپ کنید و OK را بزنید.

ارتقاء از طریق SSH
===================

پس از اتصال به ماینر از طریق SSH, ارتقا به آخرین فریم‌ور میتواند با استفاده از دستور زیر آغاز شود:

::

  opkg update && opkg install firmware

از آنجاییکه نصب فریم‌ور باعث ریبوت میشود، خروجی زیر انتظار میرود:

::

  ...
  Collected errors:
  * opkg_conf_load: Could not lock /var/lock/opkg.lock: Resource temporarily unavailable.
    Saving config files...
    Connection to 10.10.10.1 closed by remote host.
    Connection to 10.10.10.1 closed.

.. _upgrade_community_bos_plus:

**********************************
ارتقاء به نسخه تجاری Braiins OS+
**********************************

برای ارتقا از نسخه قدیمی یا نسخه آزاد به Braiins OS+، از طریق SSh به ماینر وصل شوید و دستورات زیر را اجرا کنید:

::

    opkg update && opkg install bos_plus

.. _downgrade_bos_plus_community:

**************************************
ارتقاء / بازگردانی به نسخه آزاد
**************************************

برای ارتقا از نسخه قدیمی Braiins OS یا بازگردانی از Braiins OS+ , ازطریق SSH به ماینر وصل شوید و دستور زیر را استفاده کنید (متغییر ``IP_ADDRESS`` را جایگزین کنید):

::

  ssh root@IP_ADDRESS 'wget -O /tmp/firmware.tar https://feeds.braiins-os.org/am1-s9/firmware_2020-03-29-0-6ec1a631_arm_cortex-a9_neon.tar && sysupgrade -F /tmp/firmware.tar'

.. _downgrade_bos_stock:

***********************************
Reset to initial Braiins OS version
***********************************

پکیج فریم‌ور کنونی میتواند به نسخه ای که در ابتدا هنگام جایگزینی فریم‌ور اصلی کارخانه نصب شده بود، بازگردانده شود. این کار میتواند به این صورت انجام شود

 -  *IP SET دکمه* - نگه‌داشتن دکمه برای *۱۰ ثانیه* تا زمانیکه LED قرمز چشمک بزند.
 -  *SD card* - فایل *uEnv.txt* را ویرایش کنید و کد مقابل را به **factory_reset=yes** تغییر دهید.
 -  *miner utility* - دستور ``miner factory_reset`` را در SSH اجرا کنید.
 -  *opkg package* - دستور ``opkg remove firmware`` را در SSH اجرا کنید.

***************************
Flashing a factory firmware
***************************

Using previously created backup
===============================

By default, a backup of the original firmware is created during the
migration to Braiins OS and can be restored using the following commands (replace the placeholders ``BACKUP_ID_DATE`` and ``IP_ADDRESS`` accordingly):

::

  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-0-6ec1a631 && source .env/bin/activate
  python3 restore2factory.py backup/BACKUP_ID_DATE/ IP_ADDRESS

Using factory firmware image
=============================

On an Antminer S9, you can alternatively flash a factory firmware image
from the manufacturer’s website, with ``FACTORY_IMAGE`` being file path
or URL to the ``tar.gz`` (not extracted!) file. Supported images with
corresponding MD5 hashes are listed in the
`platform.py <https://github.com/braiins/braiins-os/blob/master/upgrade/am1/platform.py>`__
file.

Run (replace the placeholders ``FACTORY_IMAGE`` and ``IP_ADDRESS`` accordingly):

::

  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-0-6ec1a631 && source .env/bin/activate
  python3 restore2factory.py --factory-image FACTORY_IMAGE IP_ADDRESS
