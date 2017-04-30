---
published: false
---
## Create a completely custom magento2 theme
This article assumes that you have a magento2 installation setup and ready. Also it assumes that you have installed magento2 using the [magento archive](http://devdocs.magento.com/guides/v2.1/install-gde/prereq/zip_install.html "installation instructions").

It will also be helpful if have you [sample data installed](http://devdocs.magento.com/guides/v2.1/install-gde/install/sample-data-after-composer.html).

### First of all copy the blank theme
Copy the black theme form `<magento_root>/vendor/magento/theme-frontend-blank/` to `<magento_root>/app/design/frontend/Developer/ThemeName/` 

Note: replace `Developer` with your or your company's name, and `ThemeName` with the name you want to give to your custom theme.

### Edit theme.xml
In the `<theme_directory>` (<magento_root>/app/design/frontend/Developer/ThemeName/) edit the title tag and put the your `ThemeName`
