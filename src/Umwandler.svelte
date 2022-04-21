<script>
import App from "./App.svelte";



    let firstNumber;
    let firstNumSys = "2";
    let firstInput;

    let secNumber;
    let secNumSys = "10";
    let secInput;
       

    /**
     * 
     * @param {string} num Number in the acc. numeral system; for exp. "AF5"
     * @param {number} numeralSystem the according numberal system; for exp. 16
     */

    function convertToDecimalSystem(num /*Type string*/, numeralSystem /*Type number*/) { 
        let dezimalNum = 0;
        const hexNumArr = ["a", "b", "c", "d", "e", "f"];

        if (num !== "") {
            num = num.replaceAll(" ", "");
        }else{
            return ""
        }

        /* Split the number into its single digits + lower case them for comparision with hexNumArr */
        let numArray = [...num.toLowerCase()] 
        /* Reverse the order of the number digits so it reads from the right to the left */
        numArray.reverse(); 


        for (let i = 0; i < numArray.length; i++) {
            
            /* For numeral systems with a base higher than 10, convert letters to the according numeral value (for now up to hexadecimal)*/
            if (hexNumArr.includes(numArray[i])) {
                numArray[i] = hexNumArr.indexOf(numArray[i]) + 10;
            }
            /* NOTE!: I didn't new about the toString() function on numbers and how Javascript treats hex values, whereas this solution 
            to convert for example a hexString back to decimal may not be optimal */


            /* Convert the single digits in the array back to real numbers */
            numArray[i] = Number(numArray[i]);

            /* Multiply each number digit by the base of the numeral system to the power of the digits position in the number, starting from 0 (i) */
            dezimalNum = dezimalNum + numArray[i] * Math.pow(numeralSystem, i);
        }
 
        /* Check if the entered number can even exist it the specified numeral system */
        if (Math.max(...numArray) + 1 > numeralSystem) {
            return "Your entered number has character with higher values than the selected numeral system";
        }
        
        return dezimalNum;
    };

    /* Converts a decimal number to any numeral system using the "Horner scheme" and outputs it as a number(string)*/
    /**
     * 
     * @param {number} num decimal number to be used in the "Horner scheme" for conversion
     * @param {number} numeralSystem the numeral system into which the decimal number is to be converted 
     */
    function convertDecimalToNumeralSystem(num, numeralSystem) {
        const hexNumArr = ["a", "b", "c", "d", "e", "f"];

        let convertedNumber = 0;
        let quotient = num;
        let remainders = [];

        /* Implementation of the "Horner scheme" to divide the decimal number(quotient) until it equals zero, while 
        saving the remainder of each division in an array, which values equal the converted number when written down 
        from right to left (bottom to top)*/
        while(quotient !== 0) {
            remainders.push(quotient % numeralSystem);
            quotient = Math.trunc(quotient / numeralSystem);
        }

        /* "Writes down" the converted number in it's right order (from right to left) as a complete number(as mentioned above in the comment at the end) */
        remainders = remainders.reverse();

        /* Convert numbers above the decimal numeral system(0-9) into the equivalent characters; for exp. 15 = F, 10 = A */
        for (let i = 0; i < remainders.length; i++) {
            if (remainders[i] > 9) {
                remainders[i] = remainders[i].toString(numeralSystem).toUpperCase(); 
            }
        }

        convertedNumber = remainders.join("")

        return convertedNumber;
    }

    function convertToAnyNumeralSystem(num, numeralSystem, wishNumeralSystem) {
        let decimalNumber = convertToDecimalSystem(num, numeralSystem);

        if (typeof(decimalNumber) === "number") {
            return convertDecimalToNumeralSystem(decimalNumber, wishNumeralSystem);
        }else if (decimalNumber === "") {
            return ""
        }else {
            return "Fehler"
        }

    }

</script>

<style>

</style>

<div>

    <select id="firstNumSystem" bind:value={firstNumSys} on:change={secInput.value = convertToAnyNumeralSystem(firstNumber, firstNumSys, secNumSys)}>
        <option value="2">2 (Binär)</option>
        <option value="3">3</option>
        <option value="4">4</option>
        <option value="5">5</option>
        <option value="6">6</option>
        <option value="7">7</option>
        <option value="8">8</option>
        <option value="9">9</option>
        <option value="10">10 (Dezimal)</option>
        <option value="11">11</option>
        <option value="12">12</option>
        <option value="13">13</option>
        <option value="14">14</option>
        <option value="15">15</option>
        <option value="16">16 (Hexadezimal)</option>
    </select>
    <input type="text" name="firstNum" id="firstNum" bind:this={firstInput} bind:value={firstNumber} on:input={secInput.value = convertToAnyNumeralSystem(firstNumber, firstNumSys, secNumSys)}>
    


    <select id="secNumSystem" bind:value={secNumSys} on:change={firstInput.value = convertToAnyNumeralSystem(secNumber, secNumSys, firstNumSys)}>
        <option value="2">2 (Binär)</option>
        <option value="3">3</option>
        <option value="4">4</option>
        <option value="5">5</option>
        <option value="6">6</option>
        <option value="7">7</option>
        <option value="8">8</option>
        <option value="9">9</option>
        <option value="10">10 (Dezimal)</option>
        <option value="11">11</option>
        <option value="12">12</option>
        <option value="13">13</option>
        <option value="14">14</option>
        <option value="15">15</option>
        <option value="16">16 (Hexadezimal)</option>
    </select>
    <input type="text" name="secNum" id="secNum" bind:this={secInput} bind:value={secNumber} on:input={firstInput.value = convertToAnyNumeralSystem(secNumber, secNumSys, firstNumSys)}>

</div>


<!-- for (let i = 0; i < numArray.length; i++) {

    
    switch(numArray[i]) {
        case "A":
            numArray[i] = "10";
            break;
        case "B":
            numArray[i] = "11";
            break;
        case "C":
            numArray[i] = "12";
            break;
        case "D":
            numArray[i] = "13";
            break;
        case "E":
            numArray[i] = "14";
            break;
        case "F":
            numArray[i] = "15";
            break;
        default:
            break;
    }

    
    numArray[i] = Number(numArray[i]); 

    
    dezimalNum = dezimalNum + numArray[i] * Math.pow(numeralSystem, i);
}  -->
