# WooCommerce Bogo Plugin
A very simple plugin to enable Buy One Get One 50% Off for WooCommerce

It doesn't have a dashboard so it needs editing in the code. `$target_products` are the product IDs of the products you want it applied to, and the `$product_limit` is the quantity of products you'll allow the promo.

```
<?php
/**
 * Plugin Name: BOGO 50% Off WooCommerce
 * Description: A simple WooCommerce plugin to enable Buy One Get One 50% Off deals.
 * Version: 1.0
 * Author: ChatGPT
 */

// Apply the discount
add_action('woocommerce_cart_calculate_fees', 'bogo_fifty_percent_off', 10, 1);
function bogo_fifty_percent_off($cart) {
    if (is_admin() && !defined('DOING_AJAX')) {
        return;
    }

    $target_products = array(1, 2, 3); // Replace with your product IDs
    $product_limit = 1; // Set your product limit here
    $discount = 0;

    foreach ($cart->get_cart() as $cart_item) {
        if (in_array($cart_item['product_id'], $target_products)) {
            $qty = min($cart_item['quantity'], $product_limit);
            $product_limit -= $qty;
            $discount -= $qty * ($cart_item['data']->get_price() * 0.5);
            if ($product_limit <= 0) break;
        }
    }

    if ($discount < 0) {
        $cart->add_fee(__('BOGO 50% Off Discount'), $discount);
    }
}
```
