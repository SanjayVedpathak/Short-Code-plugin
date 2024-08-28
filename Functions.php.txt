// Remove feed URLs from the header

function disable_feed_links() {
remove_action('wp_head', 'feed_links', 2);
remove_action('wp_head', 'feed_links_extra', 3);
}
add_action('init', 'disable_feed_links');
// Redirect feed requests to the original page
function redirect_feed_requests_to_original_page($query) {
if ($query->is_feed) {
global $wp;
$current_url = home_url(add_query_arg(array(), $wp->request));
$original_url = preg_replace('/\/feed(\/.*|$)/', '', $current_url);
wp_redirect($original_url, 301);
exit;
}
}
add_action('parse_query', 'redirect_feed_requests_to_original_page');
