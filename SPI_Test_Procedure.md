# Procedure for Testing Inter-Board SPI Communication

## Part 1: Setup
 * Power two SAM D51 breakout boards from the same 3V3 power supply.
 * Load the BruinSpace CircuitPython firmware onto the cdh test board (CDH) and the subsystem test board (TST) using the UF2 bootloader.
 * Load both the files in the `flight_computer` folder and the entire `lib` folder onto the cdh test board and open a serial monitor on the laptop connected to the board.
 * Load the files in the `testing_board` folder and the entire `lib` folder onto the subsystem test board and open a serial monitor on the laptop connected to the board.
 * Verify that the cdh test board is printing out simulated sensor values as X in "CDH wrote X to SPI" and printing out zeros as Y in "CDH read Y from SPI" and that the subsystem test board is printing out simulated sensor values as X in "TST wrote X to SPI" and printing out zeros as Y in "TST read Y from SPI".

## Part 2: Validate Two-Way Communication
 * Ensure that both boards are using the default SPI (SERCOM2) by making sure the pin parameters in the constructor on CDH match
 `busio.SPI(clock=board.SCK, MOSI=board.MOSI, MISO=board.MISO)` and that the pin parameters in the constructor on TST match
 `busio.SPI(board.SCK, MOSI=board.MOSI, MISO=microcontroller.pin.PA15, SS=board.MISO, slave_mode=True)`.
 * Connect the data signals of the boards according to the following table:

    | Signal &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | CDH Pin &nbsp;&nbsp;&nbsp; | TST Pin &nbsp;&nbsp;&nbsp; |
    | ----- | ----- | ----- |
    | MOSI | 29 | 29 |
    | SCK | 30 | 30 |
    | MISO | 31 | 32 |
    | SS | 40 | 31 |

 * Verify that both boards are printing out simulated sensor values as X in "Wrote X to SPI" and printing out the simulated sensor values of
 the other board as Y in "Read Y from SPI".

## Part 3: Validate Controller Mode on Alternate SERCOMs
 * Keep the subsystem test board on the default SPI (SERCOM2) by making sure the pin parameters in the constructor match
 `busio.SPI(clock=board.SCK, MOSI=board.MOSI, MISO=board.MISO, SS=microcontroller.pin.PA15)`.
 * Move the flight computer test board to SPI on SERCOM1 by making the pin parameters in the constructor match
 `busio.SPI(clock=board.D12, MOSI=board.D13, MISO=board.D10)`.
 * Connect the data signals of the boards according to the following table:

    | Signal &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | CDH Pin &nbsp;&nbsp;&nbsp; | TST Pin &nbsp;&nbsp;&nbsp; |
    | ----- | ----- | ----- |
    | MOSI | 35 | 29 |
    | SCK | 36 | 30 |
    | MISO | 37 | 32 |
    | SS | 40 | 31 |

 * Verify that both boards are printing out simulated sensor values as X in "Wrote X to SPI" and printing out the simulated sensor values of
 the other board as Y in "Read Y from SPI".

## Part 4: Validate Peripheral Mode on Alternate SERCOMs
 * Keep the flight computer test board on SERCOM1 for SPI by making sure the pin parameters in the constructor match
 `busio.SPI(clock=board.D12, MOSI=board.D13, MISO=board.D10)`.
 * Move the subsystem test board to SPI on SERCOM1 by making the pin parameters in the constructor match
 `busio.SPI(clock=board.D12, MOSI=board.D13, MISO=board.D11, SS=board.D10)`.
 * Connect the data signals of the boards according to the following table:

    | Signal &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | CDH Pin &nbsp;&nbsp;&nbsp; | TST Pin &nbsp;&nbsp;&nbsp; |
    | ----- | ----- | ----- |
    | MOSI | 35 | 35 |
    | SCK | 36 | 36 |
    | MISO | 37 | 38 |
    | SS | 40 | 37 |

 * Verify that both boards are printing out simulated sensor values as X in "Wrote X to SPI" and printing out the simulated sensor values of
 the other board as Y in "Read Y from SPI".
