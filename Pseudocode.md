Ice Making Machine

//START

INIT();

DETERMINE WaterLVL

DETERMINE IceLVL

INPUT DispenserSwitchState

WHILE WaterLVL > 0 AND IceLVL < 100

    ICEMAKE()

ELSE

    PRINT ERROR - Operation has ceased. Please check water/ice levels.

    END PROGRAM

ENDWHILE

IF DispenserSwitchState = Crushed

    CRUSH()

Else

    END PROGRAM

ENDIF

END

//Initialize Values
INIT function
    CREATE Temp //Celsius
    SET SwitchState := 0 //Default Condenser Switch State - Set to cool
    CREATE IceLVL 
    CREATE WaterLVL
    CREATE Fan() //Internal cooling fan used at start
    CREATE DispenserSwitchState //Input value for Cubed or Crushed Ice
    CREATE Count //counter variable

//General code for icemaking program
ICEMAKE function
    DETERMINE Temp
    SET Count := 0
    FOR IceLVL < 100
        IF Temp > -5
            Fan()
            IF Count > 3
                PRINT Error - Ice Making cannot start - Check internal fan and temperature
                END PROGRAM
            ENDIF
        ENDIF
        Pump()
        Condense()
        CALCULATE IceLVL
    ENDFOR
RETURN

//code for fan function
FAN function
    FOR 600 seconds
        Activate cooling fan for internal system //Fan cools system internally, then evaluates temp after 600 secs, adds to counter
        CALCULATE Temp
        Count++
    ENDFOR
RETURN

//code for cooling pump function
PUMP function
    FOR 120 seconds
        Pump condensed refrigerant into evaporation chamber //liquid refrigerant turns into a gas, absorbing heat and freezing ice in layers
    ENDFOR
RETURN

//code for heat condense function
CONDENSE function
    SET SwitchState := 1 //changes condenser to heat instead of cool
    FOR 15 seconds
        Pump evaporated refrigerant back into condenser //copper pipes heat with the evaporated refrigerant, causing ice to melt and slide off molding grid, dumping into the collection bin below
    ENDFOR
RETURN

//code for crushed ice function
CRUSH function
    FOR 15 seconds
        Activate crush roller for top layer
    ENDFOR
    Dispense crush ice from top layer
RETURN
    
    


        


        

   

