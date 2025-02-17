# Resto.Front.Api.UDSTestPaymentPlugin
Внесение изменений относительно оригинального плагина из примера.
Resto.Front.Api.UDSTestPaymentPlugin/ExternalPaymentProcessorSample.cs

#### ExternalPaymentProcessorSample.cs

```C#
public void OnPaymentAdded(
    [NotNull] IOrder order,
    [NotNull] IPaymentItem paymentItem,
    [NotNull] IUser cashier,
    [NotNull] IOperationService operations,
    IReceiptPrinter printer,
    [NotNull] IViewManager viewManager,
    IPaymentDataContext context)
{
    switch (order.Status)
    {
        case OrderStatus.New:
            var credentials = operations.GetDefaultCredentials();
            var allDiscTypes = operations.GetDiscountTypes();
            var discount = allDiscTypes.FirstOrDefault(x => x.Name.ToLower().Contains("uds")
                && x.DiscountByFlexibleSum);

            if (order.FullSum >= 1000 && discount != null)
            {
                var discountAmount = order.FullSum * 0.10m; // Скидка 10%
                operations.AddFlexibleSumDiscount(discountAmount,discount, order, credentials);
                PluginContext.Log.Info($"Applied 10% discount to order ID {order.Id}. Discount amount: {discountAmount}");
            }
            return;
        default:
            operations.AddStatusBarInfo("Все скидки раcсчитаны");
            return;
    }
}
```