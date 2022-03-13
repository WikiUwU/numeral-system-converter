<script>

    let firstNumber;
    let firstNumSys = "2";

    let secNumber;
    let secNumSys = "10";
    


    /**
     * 
     * @param {string} num Number in the acc. numeral system; for exp. "AF5"
     * @param {number} numeralSystem the according numberal system; for exp. 16
     */

    function convertToDecimalSystem(num /*Type string*/, numeralSystem /*Type number*/) { 
        let dezimalNum = 0;
        let numberLettersRegex = /[A-F]/

        /* Split the number into its single digits */
        let numArray = [...num] 
        /* Reverse the order of the number digits so it reads from the right to the left */
        numArray.reverse(); 

        for (let i = 0; i < numArray.length; i++) {

            /* For numeral systems with a base higher than 10, convert letters to the according numeral value (for now up to hexadecimal)*/
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


</script>

<style>

</style>

<div>

    <select id="firstNumSystem" bind:value={firstNumSys}>
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
    <input type="text" name="firstNum" id="firstNum" bind:value={firstNumber} on:input={secNum.value = convertToDecimalSystem(firstNumber, firstNumSys)}>
    <!-- <p on:click={console.log(secNumber,typeof secNumber)}>"Hello {secNumber}"</p> -->
    


    <select id="secNumSystem" bind:value={secNumSys}>
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
    <input type="text" name="secNum" id="secNum" bind:value={secNumber}>

</div>
