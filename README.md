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
