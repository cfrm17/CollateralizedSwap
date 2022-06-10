# Collateralized Swap

The Collateralized swap structure is an option where the client, rather than making an upfront cash payment, puts up collateral instead–in this case in the form of a basket of hedge fund investments. For the most part the option acts as an equity swap, with the client paying the returns on a basket of hedge funds and receiving a spread over Libor, with the notional amount resetting periodically.

The value of the collateral affects the current leverage ratio and can trigger re-/de-leveraging or redemption, but otherwise has no affect on the option valuation itself. Nevertheless, to monitor the leverage ratio daily the collateral needs to be MTM daily, and ultimately the dealer’s risk in a situation where the deal must be closed out depends on the closeout value of the collateral.

Therefore, although as far and the daily P&L process is concerned, these options will look quite similar to an accreting strike option, from a risk standpoint 
they are quite different. In particular, a VaR calculation would need to consider the closeout volatility of the collateral as well as the basket, and should
be vetted separately from the calculation performed for other options.


The underlying equity consists of a basket of hedge funds (and cash and potentially other securities). At any time t within any valuation period starting at T 
(which are year long in this case), the ‘Equity Amount’ is the sum of dollar changes in basket value since the beginning of the valuation period: Et = ΣΔB, 
with the sum taken over months since the beginning of the valuation period.

Note that an Additional Capital Balance (positive from a re-leveraging and negative from a de-leveraging) will increase or decrease the basket value at any time, 
but will not directly alter the Equity Amount. It does affect the magnitude of _B as it acts like a larger or smaller investment in the basket, that is, you will 
have a dollar change amount: ΔB = (B0 + ACB) R where R is the return on the basket.

The calculation of the floating payments are performed monthly, with a notional amount N that resets monthly: Nt = N0 + Et, that is, it grows with the dollar 
growth of the basket, beginning at the starting basket level N0 = B0.

The ‘Floating Amount’ for each month is similar to the fees earned for the more vanilla options. Note that in this case, the fees are based on the end of month 
values, so the fee charged at the end of month is based on Nt which includes the current month’s return, and Bend is the end of month basket value, so the s(= 1.5%) 
spread fee is charged as a fraction of the month-end basket value. (Therefore, the fees that accrue daily will float with changes to the basket value, rather than 
accruing a fixed amount known beforehand, as is common in other options.)

The final contribution TU arises from a variety of sources: termination fees, fees to make changes to the basket, and a ‘true up’ amount (related to the termination 
of a previous contract and initiation of the current one).

The cumulative floating amount is calculated at each month-end as the current floating amount plus the cumulative floating amount at the end of the previous month 
with interest of (Libor + s) act/360. The cumulative floating amount resets to zero at the end of each equity valuation period.

At the end of every equity amount valuation period, the net of the equity amount and the cumulative floating payments is cash settled–the difference to RBC if 
the floating payments are larger, to the counterparty if the Equity payments are larger. RBC makes payments to the client’s collateral account, where it remains, 
and takes payments either from the collateral account or the basket.

The client provides collateral to support the option so that the initial leverage ratio (hedge fund balance / collateral) is ~ 3. At any later date, 
the “adjusted credit support” is calculated as the current collateral value, plus equity less cumulative floating amounts, less any amount owing to the party 
from prior periods. The leverage ratio has a target of 3.0, and the deal re-levers at 2.67, and de-levers at 3.5.

Note that leverage includes only hedge fund investments, so that redeeming from a fund and retaining the redeemed cash in the basket will reduce the leverage ratio. 
De-leveraging is accomplished by the client posting additional collateral to reset L to the target, and if the client does not the deal is unwound. Re-leveraging 
is accomplished by allocating cash in the basket to hedge funds, or by the client providing ‘additional capital’ to be allocated.


Reference:

https://finpricing.com/lib/EqSpread.html

https://zenodo.org/record/6539697/files/zenodo-CollateralizedSwap.pdf

https://zenodo.org/record/6539697#.YpDuTKgpDq4

