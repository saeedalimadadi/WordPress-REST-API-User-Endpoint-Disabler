# WordPress REST API User Endpoint Disabler

This code snippet provides a solution to **disable the WordPress REST API user endpoints**. By default, the WordPress REST API exposes user data (such as usernames, display names, and sometimes even email hashes) through `/wp/v2/users` and individual user endpoints like `/wp/v2/users/1`. Disabling these endpoints can be a significant security measure, especially for sites where user data privacy is critical or where user enumeration is a concern.

## Why disable the REST API user endpoints?

* **Security Hardening:** Prevents malicious actors from enumerating usernames or gathering information about your site's users, which can be used in brute-force attacks or other exploits.
* **Privacy:** If your site doesn't require public user data exposure via the REST API, disabling these endpoints helps protect user privacy.
* **Reduced Attack Surface:** Limits the amount of information publicly available about your WordPress installation.

## Features

* **Disables the `/wp/v2/users` endpoint:** Prevents access to a list of all users.
* **Disables individual user endpoints (`/wp/v2/users/(?P<id>[\d]+)`):** Prevents access to specific user profiles by ID.
* **Enhances Privacy & Security:** Reduces the risk of user enumeration and data exposure.

## Installation

There are two primary methods to implement this code:

### Method 1: As a Standalone Plugin (Recommended)

Creating a small, dedicated plugin is the most robust way to add this functionality. It ensures your modification remains active regardless of theme changes.

1.  Create a new folder named `wp-rest-no-users` inside your WordPress site's `wp-content/plugins/` directory.
2.  Inside this new folder, create a file named `wp-rest-no-users.php`.
3.  Copy and paste the following code into `wp-rest-no-users.php`:

    ```php
    <?php
    /**
     * Plugin Name: WordPress REST API User Endpoint Disabler
     * Description: Disables the /wp/v2/users REST API endpoint to enhance security and user privacy.
     * Version: 1.0
     * Author: Your Name (Optional)
     * License: GPL-2.0-or-later
     */

    if ( ! defined( 'ABSPATH' ) ) {
        exit; // Exit if accessed directly.
    }

    add_filter('rest_endpoints', function( $endpoints ) {
        // Unset the endpoint for all users
        if ( isset( $endpoints['/wp/v2/users'] ) ) {
            unset( $endpoints['/wp/v2/users'] );
        }
        // Unset the endpoint for individual users by ID
        if ( isset( $endpoints['/wp/v2/users/(?P<id>[\d]+)'] ) ) {
            unset( $endpoints['/wp/v2/users/(?P<id>[\d]+)'] );
        }
        return $endpoints;
    });
    ?>
    ```

4.  Go to your WordPress admin dashboard, navigate to **"Plugins"**, and **activate** the "WordPress REST API User Endpoint Disabler" plugin.

### Method 2: Adding to your Theme's functions.php File

You can add this code directly to your active theme's `functions.php` file. **Before doing so, it's highly recommended to back up your `functions.php` file.**

1.  Navigate to `wp-content/themes/YourThemeName/` (replace `YourThemeName` with the actual name of your active theme).
2.  Open the `functions.php` file.
3.  Add the following code to the end of the file (before the closing `?>` tag, if one exists):

    ```php
    add_filter('rest_endpoints', function( $endpoints ) {
        // Unset the endpoint for all users
        if ( isset( $endpoints['/wp/v2/users'] ) ) {
            unset( $endpoints['/wp/v2/users'] );
        }
        // Unset the endpoint for individual users by ID
        if ( isset( $endpoints['/wp/v2/users/(?P<id>[\d]+)'] ) ) {
            unset( $endpoints['/wp/v2/users/(?P<id>[\d]+)'] );
        }
        return $endpoints;
    });
    ```

## Important Considerations

* **Potential Side Effects:** While generally safe for most websites, some plugins or themes might rely on these specific REST API endpoints to fetch user data on the frontend. Test your website thoroughly after implementing this code to ensure all functionalities work as expected.
* **User Enumeration in Other Ways:** Disabling the REST API user endpoint is a great step, but attackers might still try to enumerate users through other methods (e.g., author archives, login error messages if not generic). Combine this with other security measures for comprehensive protection.
* **Reversibility:** To re-enable these endpoints, simply deactivate or remove this plugin/code.

## Contributing

Contributions are welcome! If you have suggestions or improvements for this code, feel free to open a "Pull Request" or report an "Issue."

## License

This project is licensed under the GPL-2.0-or-later License.
---

# غیرفعال‌کننده نقطه پایانی کاربران REST API وردپرس

این قطعه کد راه حلی برای **غیرفعال کردن نقطه پایانی کاربران REST API وردپرس** ارائه می‌دهد. به طور پیش‌فرض، REST API وردپرس داده‌های کاربر (مانند نام‌های کاربری، نام‌های نمایشی و گاهی حتی هش ایمیل) را از طریق `wp/v2/users/` و نقاط پایانی کاربران فردی مانند `wp/v2/users/(?P<id>[\d]+)` در دسترس قرار می‌دهد. غیرفعال کردن این نقاط پایانی می‌تواند یک اقدام امنیتی مهم باشد، به ویژه برای سایت‌هایی که حفظ حریم خصوصی داده‌های کاربر حیاتی است یا نگرانی‌هایی در مورد شمارش کاربران وجود دارد.

## چرا نقطه پایانی کاربران REST API را غیرفعال کنیم؟

* **افزایش امنیت:** از شمارش نام‌های کاربری یا جمع‌آوری اطلاعات درباره کاربران سایت شما توسط مهاجمان مخرب جلوگیری می‌کند، که می‌تواند در حملات brute-force یا سایر بهره‌برداری‌ها استفاده شود.
* **حفظ حریم خصوصی:** اگر سایت شما نیازی به افشای عمومی داده‌های کاربر از طریق REST API ندارد، غیرفعال کردن این نقاط پایانی به محافظت از حریم خصوصی کاربر کمک می‌کند.
* **کاهش سطح حمله:** میزان اطلاعات عمومی موجود درباره نصب وردپرس شما را محدود می‌کند.

## قابلیت‌ها

* **غیرفعال کردن نقطه پایانی `wp/v2/users/`:** از دسترسی به لیست همه کاربران جلوگیری می‌کند.
* **غیرفعال کردن نقاط پایانی کاربران فردی (`wp/v2/users/(?P<id>[\d]+)`):** از دسترسی به پروفایل‌های کاربران خاص با شناسه جلوگیری می‌کند.
* **افزایش حریم خصوصی و امنیت:** خطر شمارش کاربر و افشای داده‌ها را کاهش می‌دهد.

## نصب

برای پیاده‌سازی این کد، دو روش اصلی وجود دارد:

### روش ۱: به عنوان یک افزونه مستقل (توصیه شده)

ایجاد یک افزونه کوچک و اختصاصی، قوی‌ترین راه برای افزودن این قابلیت است. این تضمین می‌کند که تغییر شما صرف‌نظر از تغییر قالب، فعال باقی بماند.

1.  یک پوشه جدید با نام `wp-rest-no-users` در مسیر `wp-content/plugins/` سایت وردپرسی خود ایجاد کنید.
2.  در داخل این پوشه جدید، یک فایل با نام `wp-rest-no-users.php` ایجاد کنید.
3.  کد زیر را در `wp-rest-no-users.php` کپی و جایگذاری کنید:

    ```php
    <?php
    /**
     * Plugin Name: WordPress REST API User Endpoint Disabler
     * Description: Disables the /wp/v2/users REST API endpoint to enhance security and user privacy.
     * Version: 1.0
     * Author: Your Name (Optional)
     * License: GPL-2.0-or-later
     */

    if ( ! defined( 'ABSPATH' ) ) {
        exit; // Exit if accessed directly.
    }

    add_filter('rest_endpoints', function( $endpoints ) {
        // حذف نقطه پایانی برای همه کاربران
        if ( isset( $endpoints['/wp/v2/users'] ) ) {
            unset( $endpoints['/wp/v2/users'] );
        }
        // حذف نقطه پایانی برای کاربران فردی با شناسه
        if ( isset( $endpoints['/wp/v2/users/(?P<id>[\d]+)'] ) ) {
            unset( $endpoints['/wp/v2/users/(?P<id>[\d]+)'] );
        }
        return $endpoints;
    });
    ?>
    ```

4.  وارد پنل مدیریت وردپرس خود شوید، به بخش **"افزونه‌ها"** بروید و افزونه **"غیرفعال‌کننده نقطه پایانی کاربران REST API وردپرس"** را **فعال کنید**.

### روش ۲: اضافه کردن به فایل functions.php قالب شما

می‌توانید این کد را مستقیماً به فایل `functions.php` قالب فعال خود اضافه کنید. **پیشنهاد اکید می‌شود قبل از انجام این کار، از فایل `functions.php` خود یک پشتیبان (backup) تهیه کنید.**

1.  به مسیر `wp-content/themes/YourThemeName/` بروید (به جای `YourThemeName` نام واقعی قالب فعال خود را قرار دهید).
2.  فایل `functions.php` را باز کنید.
3.  کد زیر را به انتهای فایل (قبل از تگ بستن `?>`، در صورت وجود) اضافه کنید:

    ```php
    add_filter('rest_endpoints', function( $endpoints ) {
        // حذف نقطه پایانی برای همه کاربران
        if ( isset( $endpoints['/wp/v2/users'] ) ) {
            unset( $endpoints['/wp/v2/users'] );
        }
        // حذف نقطه پایانی برای کاربران فردی با شناسه
        if ( isset( $endpoints['/wp/v2/users/(?P<id>[\d]+)'] ) ) {
            unset( $endpoints['/wp/v2/users/(?P<id>[\d]+)'] );
        }
        return $endpoints;
    });
    ```

## ملاحظات مهم

* **عوارض جانبی احتمالی:** در حالی که این کد به طور کلی برای اکثر وب‌سایت‌ها ایمن است، برخی افزونه‌ها یا قالب‌ها ممکن است برای واکشی داده‌های کاربر در فرانت‌اند به این نقاط پایانی REST API خاص متکی باشند. پس از پیاده‌سازی این کد، وب‌سایت خود را به طور کامل آزمایش کنید تا از عملکرد صحیح همه قابلیت‌ها اطمینان حاصل کنید.
* **شمارش کاربر از طرق دیگر:** غیرفعال کردن نقطه پایانی کاربران REST API یک گام عالی است، اما مهاجمان ممکن است همچنان تلاش کنند کاربران را از طریق روش‌های دیگر (مانند آرشیو نویسندگان، پیام‌های خطای ورود در صورت عدم عمومی بودن) شمارش کنند. این را با سایر اقدامات امنیتی برای محافظت جامع ترکیب کنید.
* **قابلیت بازگشت:** برای فعال‌سازی مجدد این نقاط پایانی، کافیست این افزونه/کد را غیرفعال یا حذف کنید.

## مشارکت (Contributing)

مشارکت شما خوشایند است! اگر پیشنهاد یا بهبودهایی برای این کد دارید، می‌توانید یک "Pull Request" ایجاد کنید یا "Issue" جدیدی را گزارش دهید.

## مجوز (License)

این پروژه تحت مجوز GPL-2.0-or-later منتشر شده است.
