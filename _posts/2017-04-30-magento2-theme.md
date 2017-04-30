---
published: false
---
## Create a completely custom magento2 theme
This article assumes that you have a magento2 installation setup and ready. Also it assumes that you have installed magento2 using the [magento archive](http://devdocs.magento.com/guides/v2.1/install-gde/prereq/zip_install.html "installation instructions").

It will also be helpful if have you [sample data installed](http://devdocs.magento.com/guides/v2.1/install-gde/install/sample-data-after-composer.html).

### 1. First of all copy the blank theme
Copy the black theme form `<magento_root>/vendor/magento/theme-frontend-blank/` to `<magento_root>/app/design/frontend/Developer/ThemeName/` 

_Note: replace `Developer` with your or your company's name, and `ThemeName` with the name you want to give to your custom theme._

### 2. Edit theme.xml
In the `<theme_directory>` (`<magento_root>/app/design/frontend/Developer/ThemeName/`) edit the title tag and put the your `ThemeName`

### 3. Edit registration.php
Edit the `<theme_directory>\registration.php` and make sure it looks someting like this:
```php
<?php
\Magento\Framework\Component\ComponentRegistrar::register(
	\Magento\Framework\Component\ComponentRegistrar::THEME,
	'frontend/Developer/ThemeName',
	__DIR__
);
```

### 4. Remove the default css
The first thing we are going to do is remove any default css files included by the blank theme. We will leave print.css and calender.css as it is.

To remove `styles-l.css` and `styles-m.css` include the following lines in the head section of the file `<theme_directory>\Magento_Theme\layout\default_head_blocks.xml`

```xml
<remove src="css/styles-m.css" />
<remove src="css/styles-l.css" />
```

_Note: If you have installed sample data, then there will be one more styles.css included. To remove it go to the admin panel, `Content -> Configuration -> Edit`  and then make sure the Scripts and Style Sheets field in the HTML Head section is blank_