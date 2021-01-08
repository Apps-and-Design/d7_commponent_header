ASU COMPONENT HEADER MODULE FOR DRUPAL 7, WEB STANDARDS 2.0

DESCRIPTION
--------------------------
Module to add a Web Standards 2 Preact-based Header component as a block. Also 
provides a static, basic HTML and CSS Web Standards 2 Footer.

This is an alternative method of adding the ASU Web Standards 2.0 Header and 
Footer to a Drupal 7 site that doesn't depend on Webspark or a Webspark-related
theme. If your site is not built on Webspark or is but includes extensive 
customization, this module is intended to allow you to drop in the updated 
header and footer with only minor tweaking of HTML and CSS in your theme.


INSTALLATION
--------------------------

Standard Drupal module installation. All dependencies are packaged within the 
module, no further libraries to download.


USAGE
--------------------------
Once installed, visit the blocks admin page and edit the ASU Component Header 
and ASU Component Footer blocks and set their titles to "<none>". 
Additionally, edit the configuration values per your needs for the header block:
- Site title (Defaults to the Drupal site name)
- Parent Unit Name (Optional. Will display in the header)
- Parent Department URL  (Optional. Will provide link for Parent Unit Name)
- Site Menu Injection (Deliver your site's main menu through the header. Check 
  to enable, then select the Drupal menu to use.)
- Login URL and Logout URL (Defaults to standard CAS login/out paths. If your 
  site doesn't use CAS, you could give the standard Drupal paths.)

Once the blocks are configured to your liking, assign them to regions in your 
theme and disable the old header/footer blocks they are replacing.

Caveats:
- This module is currently Alpha quality and bugs or issues are possible.
- If your site uses the Administration Menu module for site Admins, it will 
  overlap the ASU Component Header. Resolving this is outside of the scope 
  of this module. In most cases the Administration Menu is only visible for 
  site Admins and won't be an issue for general users.
- Web Standards 2.0 menus only support Primary and Secondary levels. As a 
  result, the menu component doesn't support tertiary fly-outs. Tertiary menu 
  items will be ignored.
- The Header Component uses FontAwesome 5 via the SVG with Javascript 
  invocation method, and this may lead to issues with Font Awesome icons 
  employed in your site. In detail: The SVG with Javascript Font Awesome 
  triggers a procedure that scans the DOM and finds Font Awesome tags and 
  converts them on the fly to SVG tags. If you have CSS that targets the tags, 
  it may need to be adjusted. Additionally, if your site was on an older 
  version of Font Awesome, you may need to update the FA tag names to match 
  Font Awesome 5 naming conventions. Lastly, deploying icons via PHP pseudo 
  classes (::before and ::after) and the CSS content attribute, in our 
  experience, breaks and you may need to find another means of injecting the 
  icon. Eg. If you use ::before to add an icon to a Drupal menu item, you could 
  instead use a menu link theme hook in your theme's template.php file, like 
  so (but updated for your use case):

/**
 * Implement [themename]_menu_link__[menu-name]() to add lock icon into
 * my-menu menu. Requred due to pseudo elements not working
 * with Preact Header's SVG Font Awesome import.
 */
function mytheme_menu_link__my_menu(array $variables) {
  $element = $variables['element'];
  $sub_menu = '';

  if ($element['#below']) {
    $sub_menu = drupal_render($element['#below']);
  }
  $output = l($element['#title'], $element['#href'], $element['#localized_options']);

  // Catch the "Students" menu item and add an icon, since we can't use pseudo
  // classes with the SVG Font Awesome conflicts from the header.
  if ($element['#title'] == 'Students') {
    return '<li' . drupal_attributes($element['#attributes']) . '>' . '<i class="fas fa-lock"></i>' . $output . $sub_menu . "</li>\n";
  }
  else {
    return '<li' . drupal_attributes($element['#attributes']) . '>' . $output . $sub_menu . "</li>\n";
  }
}


DEPENDENCIES
--------------------------

DRUPAL MODULES
 - cas


PERMISSIONS
--------------------------
Inherits permissions from Drupal blocks system configs.


CONFIGURATION
--------------------------
See usage. All configurations exist on the blocks' UIs.


API
--------------------------
None provided by this module.


MODULES
--------------------------
No sub modules.


PAGES
--------------------------
No pages provided by this module.


BLOCKS
--------------------------
- ASU Components Header
- ASU Components Footer


HOOKS
--------------------------
None provided.
