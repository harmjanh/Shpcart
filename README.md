Shpcart
=======

Version: Beta

A Shopping Cart Bundle for Laravel, based on the Cartify bundle .
Installation


Publish Assets

php artisan asset:publish vendor/package
Now visit http://yoursite.com/shpcart and you should see the example products page.

Adding items to the Cart

Adding a single item to the Cart

To add an item to the shopping cart, simply pass an array with the product information to the Shpcart::cart()->insert() method, as shown below:

$item = array(
    'id'      => 'sku_123ABC',
    'qty'     => 1,
    'price'   => 39.95,
    'name'    => 'T-Shirt',
    'options' => array(
        'size'  => 'L',
        'color' => 'Red'
    )
);

Shpcart::cart()->insert($item);
Adding multiple items to the Cart

By using a multi-dimensional array, as shown below, it is possible to add multiple items to the cart in one action. This is useful in cases where you wish to allow people to select from among several items on the same page.

$items = array(
    array(
        'id'      => 'sku_123ABC',
        'qty'     => 1,
        'price'   => 39.95,
        'name'    => 'T-Shirt',
        'options' => array(
            'size'  => 'L',
            'color' => 'Red'
        )
    ),
    array(
        'id'    => 'sku_567ZYX',
        'qty'   => 1,
        'price' => 9.95,
        'name'  => 'Coffee Mug'
    ),
    array(
        'id'    => 'sku_965QRS',
        'qty'   => 1,
        'price' => 29.95,
        'name'  => 'Shot Glass'
    )
);

Shpcart::cart()->insert($items);
Important:

The first four array indexes above (id, qty, price, and name) are required.
If you omit any of them the data will not be saved to the cart.
The fifth index (options) is optional.
It is intended to be used in cases where your product has options associated with it.
Use an array for options, as shown above.
The five reserved indexes are:

id - Each product in your store must have a unique identifier. Typically this will be an "sku" or other such identifier.
qty - The quantity being purchased.
price - The price of the item.
name - The name of the item.
options - Any additional attributes that are needed to identify the product. These must be passed via an array.
In addition to the five indexes above, there are two reserved words: rowid and subtotal. These are used internally by the Cart class, so please do NOT use those words as index names when inserting data into the cart.

Your array may contain additional data. Anything you include in your array will be stored in the session. However, it is best to standardize your data among all your products in order to make displaying the information in a table easier.

The insert() method will return the $rowid if you successfully insert a single item.

Updating Cart items

To update the information in your cart, you must pass an array containing the Row ID and quantity to the Shpcart::cart()->update() method:

Note:

If the quantity is set to zero, the item will be removed from the cart!

Updating a single item:

$item = array(
    'rowid'   => 'b99ccdf16028f015540f341130b6d8ec',
    'qty'     => 3,
    'options' => array(
        'size'  => 'M',
        'color' => 'Red'
    )
);

Shpcart::cart()->update($item);
Updating multiple items:

$items = array(
    array(
        'rowid'   => 'b99ccdf16028f015540f341130b6d8ec',
        'qty'     => 3,
        'options' => array(
            'size'  => 'M',
            'color' => 'Red'
        )
    ),
    array(
        'rowid' => 'xw82g9q3r495893iajdh473990rikw23',
        'qty'   => 4
    ),
    array(
        'rowid'   => 'fh4kdkkkaoe30njgoe92rkdkkobec333',
        'qty'     => 2,
        'options' => array(
            'color' => 'Yellow'
        )
    )
);

Shpcart::cart()->update($items);
What is a Row ID?

The row ID is a unique identifier that is generated by the cart code when an item is added to the cart. The reason a unique ID is created is so that identical products with different options can be managed by the cart.

For example, let's say someone buys two identical t-shirts (same product ID), but in different sizes. The product ID (and other attributes) will be identical for both sizes because it's the same shirt. The only difference will be the size. The cart must therefore have a means of identifying this difference so that the two sizes of shirts can be managed independently. It does so by creating a unique "row ID" based on the product ID and any options associated with it.

In nearly all cases, updating the cart will be something the user does via the "view cart" page, so as a developer, it is unlikely that you will ever have to concern yourself with the "row ID", other then making sure your "view cart" page contains this information in a hidden form field, and making sure it gets passed to the update function when the update form is submitted. Please examine the construction of the "view cart" page below for more information.

Removing items from the Cart

To remove an item from your cart, you must pass the Row ID to the Shpcart::cart()->remove() method:

Shpcart::cart()->remove('fh4kdkkkaoe30njgoe92rkdkkobec333');
Displaying the Cart

To display the cart you will create a view file with code similar to the one shown in cart.blade.php view file.

Function Reference

Shpcart::cart()->item(rowid)
Returns all the information about an item from the shopping cart.

Shpcart::cart()->insert();
Permits you to add items to the shopping cart.

Shpcart::cart()->update();
Permits you to update items in the shopping cart.

Shpcart::cart()->remove(rowid);
Permits you to remove an item from the shopping cart.

Shpcart::cart()->total();
Displays the total amount in the cart.

Shpcart::cart()->total_items();
Displays the total number of items in the cart.

Shpcart::cart()->contents();
Returns an array containing everything in the cart.

Shpcart::cart()->has_options(rowid);
Returns TRUE (boolean) if a particular row in the cart contains options. This function is designed to be used in a loop with Cartify::cart()->contents(), since you must pass the rowid to this function, as shown in the Displaying the Cart example above.

Shpcart::cart()->item_options(rowid);
Returns an array of options for a particular item. This function is designed to be used in a loop with Cartify::cart()->contents(), since you must pass the rowid to this function, as shown in the Displaying the Cart example above.

Shpcart::cart()->destroy();
Permits you to destroy the cart. This function will likely be called when you are finished processing the customer's order.

Multiple instances of the Cart

It is possible to have multiple instances of the cart class.

You just need to pass in the name of the cart like so:

Shpcart::cart('my_other_cart')->insert($item);
It is included an Whishlist method to help you, and you can even create multiple wishlist's aswell, like so:

Shpcart::wishlist()->insert($item);
Shpcart::wishlist('my_other_wishlist')->insert($item);
Exceptions

Some methods on the Cart library throws Exceptions, below you have a list of all the Exceptions.

Shpcart\CartException
This Exception is Thrown when an unexpected error occurred.

Shpcart\CartInvalidDataException
This Exception is Thrown when an array is not passed when inserting or updating cart items.

Shpcart\CartItemNotFoundException
This Exception is Thrown when an item is not found in the shopping cart.

Shpcart\CartRequiredIndexException
This Exception is Thrown when a required index is missing from the array.

Shpcart\CartInvalidItemRowIdException
This Exception is Thrown when an Item Row ID is invalid.

Shpcart\CartInvalidItemQuantityException
This Exception is Thrown when the quantity is invalid.

Shpcart\CartInvalidItemPriceException
This Exception is Thrown when an Item Price is invalid.

To better understand on how to use the Exceptions, you can check the example controllers.
Credits

brunogaspar for writing the Cartify bundle.
Twitter for the awesome Bootstrap framework.
