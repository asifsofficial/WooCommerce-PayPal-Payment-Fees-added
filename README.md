# WooCommerce PayPal Payment Fees added

## Location:
```
functions.php
```
```
add_action( 'woocommerce_cart_calculate_fees', 'add_ppcp_transaction_fee', 10, 1 );

function add_ppcp_transaction_fee( $cart ) {
    if ( is_admin() && ! defined( 'DOING_AJAX' ) )
        return;

    // Only apply the transaction fee to ppcp-gateway payment method
    if ( WC()->session->get( 'chosen_payment_method' ) != 'ppcp-gateway' )
        return;

    $fee = ( $cart->subtotal * 0.06 ); // Calculate 6% transaction fee

    $cart->add_fee( __( 'Transaction Fee', 'woocommerce' ), $fee, true ); // Add the fee to the cart
}
```

## Updated
```
// PayPal Fee Functions
add_action( 'woocommerce_cart_calculate_fees', 'add_ppcp_transaction_fee', 10, 1 );

function add_ppcp_transaction_fee( $cart ) {
    if ( is_admin() && ! defined( 'DOING_AJAX' ) )
        return;

    // Only apply the transaction fee to ppcp-gateway payment method
    if ( WC()->session->get( 'chosen_payment_method' ) != 'ppcp-gateway' )
        return;

    $fee_percentage = get_option( 'ppcp_transaction_fee_percentage' ); // Get the transaction fee percentage from the settings field
    $fee = ( $cart->subtotal * $fee_percentage ) / 100; // Calculate the transaction fee based on the percentage

    $cart->add_fee( __( 'Transaction Fee', 'woocommerce' ), $fee, true ); // Add the fee to the cart
}

// Add a custom field for the transaction fee percentage in the WooCommerce settings page
add_filter( 'woocommerce_get_settings_checkout', 'ppcp_add_transaction_fee_percentage_field', 10, 1 );
function ppcp_add_transaction_fee_percentage_field( $settings ) {
    $settings[] = array(
        'name'     => __( 'PayPal Transaction Fee', 'woocommerce' ),
        'desc'     => __( 'Enter the percentage of the transaction fee to be applied on the checkout page', 'woocommerce' ),
        'id'       => 'ppcp_transaction_fee_percentage',
        'type'     => 'number',
        'css'      => 'min-width:300px;',
        'std'      => '0', // Set the default percentage value to .
        'desc_tip' => true,
    );
    return $settings;
}
```
