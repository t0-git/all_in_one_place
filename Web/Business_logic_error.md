## Theory

- Here, the idea is to manipulate a functionnality. You know how something works, but you try to manipulate things which shouldn't be manipulated normally.

---

## Tips

**Think outside the box. How does it work, what can I do which was not intended**
- Payment gateways or points systems are great places to look
- Look for ways to skip over steps
- Look for parameters and try to manipulate it
- Financial impact is easy to show, but others impacts exist (reputation ...)
- APIs are full of good information

---

## Basket manipulation

- Add items in your basket.
- Add another article and modify its quantity to -1.
- => Free item

---

## Order amount manipulation

- Add items to the basket
- Intercept the request before selecting a payment method
- Change the cancellation_amount parameter to 0
- => Free stuff

---

## Gaining unlimited bonus points on websites with WooCommerce Points and Rewards

- Create a new order
- Use the payment gateway Cash on Delivery
- This skips a step internally, setting a order to processing
- Processing status generates reward points
- => Free reward points

---

## Free Uber Rides

- Request a ride and intercept the request
- Edit the request, changing paymentProfileUuid parameter to a none existent uuid
- Ride disappears from profile

- Request a ride and intercept
- Change payment method parameter to a none existent payment method

---

## JSON post data

- Try to add a key value by yourself (tons of examples on fuzzdb, or payloadallthethings)

---

## Redirection after requesting a payment

- Try changing the currency in case there's only a check on the price.

---

## Refund abuse

- Purchase a product, earning points, use this points to purchase a gift, return the product to bring back your money

---



