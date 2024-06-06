<script>
    let inp = "";
    
    const months = ["january", "february", "march", "april", "may", "june", "july", "august", "september", "october", "november", "december"];
    const monthsabbr = ["jan", "feb", "mar", "apr", "may", "jun", "jul", "aug", "sep", "sept", "oct", "nov", "dec"];

    const days = ["sunday", "monday", "tuesday", "wednesday", "thursday", "friday", "saturday", "sundays", "mondays", "tuesdays", "wednesdays", "thursdays", "fridays", "saturdays"];
    const daysabbr = ["sun", "mon", "tue", "tues", "wed", "thu", "thur", "thurs", "fri", "sat"];

    class FinalDate {
        month = [];
        date = [];
        day = [];
        year = [];
        constructor(thecurrentyear) {
            this.year = [thecurrentyear, thecurrentyear];
        }
    }

    let finaloutput; 

    function handleclick(){

        finaloutput = new FinalDate(new Date().getFullYear().toString());

        inp.replaceAll(",", "");
        let text = inp.toLowerCase().split(" ");
        console.log("Input >> " + text);
        for (let each of text){
            // find months, days
            if (months.indexOf(each) >= 0){
                finaloutput.month = [...finaloutput.month, months[months.indexOf(each)]];
            } else if (monthsabbr.indexOf(each) >= 0) {
                finaloutput.month = [...finaloutput.month, monthsabbr[monthsabbr.indexOf(each)]];
            } 
            else if (days.indexOf(each) >= 0){
                finaloutput.day = [...finaloutput.day, days[days.indexOf(each) % 7]];
            } else if (daysabbr.indexOf(each) >= 0) {
                finaloutput.day = [...finaloutput.day, daysabbr[daysabbr.indexOf(each)]];
            }

        }
        console.log("Months >> " + finaloutput.month)
        console.log("Days >> " + finaloutput.day)
    }

</script>

<main>

    <input id="input" type="text" bind:value={inp} placeholder="May 4 2024 to May 4 2025 from 10 AM to 4 PM on Tuesdays">
    <input id="btn" type="button" on:click={handleclick} value="Generate">
    <br>

</main>

<style>
    main {
        width: 100%;
    }

    #input {
        padding: 20px;
        width:500px;
        height:50px;
        font-size: large;
        border-top: whitesmoke;
        border-left: whitesmoke;
        border-right: whitesmoke;
    }

    #btn {
        width:200px;
        height:91px;
        font-size: larger;
        background-color: #4aa4e9;
        color:whitesmoke;
        border:whitesmoke;
    }
</style>