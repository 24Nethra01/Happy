**GPIO**

* Not all GPIO pins can tolerate 5V(need to check before connecting) - all tolerate 3.3V
* GPIO pins can drive a max of 25mA of current
* The IDR updates every APB clk cycle hence speed of i/p streaming is given by speed of APB clk
* Output driving speed is configurable



Why BSRR when ODR can be updated directly?

ANs: To update ODR by GPIOA->ODR|=(1<<2) , we need to first read the value of ODR(word access only) then update it without affecting other bits and store back. It involves a 3 step process and if interrupted in between, it can lead to wrong data filling(can be dangerous). This happens internally because ODR is a data storage register and cannot update only one bit but the whole output value. On the other hand by GPIOA->BSRR|=(1<<2) , we can update single bit of ODR in a single indivisible step, hence called BIT ATOMIC OPERATION. It only sets or resets a particular bit in ODR without having to fetch current ODR value.



* Cortex M3,M4 have on chip hardware debug unit which allows to pause program execution and fetch instructions(breakpoint) or data(Watchpoint).
* HAL function names generally follow a pattern: HAL\_<Peripheral>\_<Action>\_<Mode>.

