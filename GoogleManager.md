# GoogleManager  
```
        function hexWithOpacity(opacityColumn,hexColumn,opacityValue,rowToStart) {
            var sheet = SpreadsheetApp.getActiveSheet();
            var range = sheet.getDataRange();
            var values = range.getValues();
            for(r = rowToStart; r <= values.length; r++) {
                var row = values[r];
                var valueText = range.getCell(r,opacityColumn).getDisplayValue();
                var percent = (Number(valueText) / 100) * 255;
                
                var hexString = DecToHex(percent);
                if(percent != 0){
                    if(range.getCell(r,hexColumn).getDisplayValue().indexOf("#") == 0 ){
                        var hexWithouthFirst =   range.getCell(r, hexColumn).getDisplayValue().slice(1);
                        var finalString = "#" + hexString.slice(-2) + hexWithouthFirst;
                        range.getCell(r,opacityValue).setValue(finalString);
                    } else {
                        var finalString = "#" + hexString.slice(-2) + range.getCell(r, hexColumn).getDisplayValue();
                        range.getCell(r,opacityValue).setValue(finalString);
                    }
                }
            }
        }
        
        function DecToHex(value) {
            var result = "";
            while( value != 0 ) {
                var temp = value % 16;
                Logger.log(temp);
                var hex = temp < 10 ? String.fromCharCode(temp+48) : String.fromCharCode(temp+55);
                result = hex.concat(result);
                value = Math.floor(value/16);
            }
            if( ( result.length %2 ) != 0 ) result = "0"+result;
            result = "0x"+result;
            return result;
        }
        
        function hexWithOpacityPrompt() {
            var ui = SpreadsheetApp.getUi();
            var response = ui.prompt('ColorManager', 'inserisci la colonna dove prendere la percentuale di opacità', ui.ButtonSet.OK_CANCEL);
            var output = ui.prompt('ColorManager', 'inserisci la colonna dove prendere gli hex', ui.ButtonSet.OK_CANCEL);
            var opacityColor = ui.prompt('ColorManager', 'inserisci la colonna dove inserire il risultato', ui.ButtonSet.OK_CANCEL);
            var rowToStart = ui.prompt('ColorManager', 'inserisci da quale riga iniziare a prendere gli hex e le percentuali', ui.ButtonSet.OK_CANCEL);
            if (rowToStart.getSelectedButton() == ui.Button.OK) {
                hexWithOpacity(response.getResponseText(),output.getResponseText(),opacityColor.getResponseText(),rowToStart.getResponseText());
            } else if (response.getSelectedButton() == ui.Button.CANCEL) {
                Logger.log('The user canceled the dialog.');
            } else {
                Logger.log('The user closed the dialog.');
            }
        }
        
        
        function getImageHeight(columnWidth,rowNumber) {
            var numberOfImage =  SpreadsheetApp.getActiveSpreadsheet().getSheets()[0].getImages();
            var sheet = SpreadsheetApp.getActiveSheet();
            var range = sheet.getDataRange();
            for(r = rowNumber; r <= numberOfImage.length; r++) {
                range.getCell(r, columnWidth).setValue(numberOfImage[r-rowNumber].getHeight());
            }
        }
        
        function getImageWidth(columnWidth,rowNumber) {
            var numberOfImage =  SpreadsheetApp.getActiveSpreadsheet().getSheets()[0].getImages();
            var sheet = SpreadsheetApp.getActiveSheet();
            var range = sheet.getDataRange();
            
            for(r = rowNumber; r <= numberOfImage.length; r++) {
                range.getCell(r, columnWidth).setValue(numberOfImage[r-rowNumber].getWidth());
            }
        }
        
        function imageHeightToPrompt() {
            var ui = SpreadsheetApp.getUi();
            var response = ui.prompt('UtilityManager', 'inserisci la colonna dove inserire le altazze delle varie immagini: ', ui.ButtonSet.OK_CANCEL);
            var rowToStart =  ui.prompt('UtilityManager', 'inserisci la riga da cui iniziare a inserire i valori: ', ui.ButtonSet.OK_CANCEL);
            if (rowToStart.getSelectedButton() == ui.Button.OK) {
                getImageHeight(response.getResponseText(),rowToStart.getResponseText());
            } else if (response.getSelectedButton() == ui.Button.CANCEL) {
                Logger.log('The user canceled the dialog.');
            } else {
                Logger.log('The user closed the dialog.');
            }
        }
        
        function imageWidthToPrompt() {
            var ui = SpreadsheetApp.getUi();
            var response = ui.prompt('UtilityManager', 'inserisci la colonna dove inserire le larghezza delle varie immagini: ', ui.ButtonSet.OK_CANCEL);
            var rowToStart =  ui.prompt('UtilityManager', 'inserisci la riga da cui iniziare a inserire i valori: ', ui.ButtonSet.OK_CANCEL);
            if (rowToStart.getSelectedButton() == ui.Button.OK) {
                getImageWidth(response.getResponseText(),rowToStart.getResponseText());
            } else if (response.getSelectedButton() == ui.Button.CANCEL) {
                Logger.log('The user canceled the dialog.');
            } else {
                Logger.log('The user closed the dialog.');
            }
        }
        
        function colorToHex(hexColumn,colorColumn,rowToStart) {
            var sheet = SpreadsheetApp.getActiveSheet();
            var range = sheet.getDataRange();
            var values = range.getValues();
            for(r = rowToStart; r <= values.length; r++) {
                var row = values[r];
                if(range.getCell(r,hexColumn).getDisplayValue().indexOf("#") == 0 ){
                    range.getCell(r,colorColumn).setBackground(range.getCell(r,hexColumn).getDisplayValue());
                } else {
                    range.getCell(r,colorColumn).setBackground("#"+range.getCell(r,hexColumn).getDisplayValue());
                }
            }
        }
        function colorToHexPrompt() {
            var ui = SpreadsheetApp.getUi();
            var response = ui.prompt('ColorManager', 'inserisci la colonna dove prendere gli hex', ui.ButtonSet.OK_CANCEL);
            var output = ui.prompt('ColorManager', 'inserisci la colonna dove inserire i colori', ui.ButtonSet.OK_CANCEL);
            var rowToStart = ui.prompt('ColorManager', 'inserisci da quale riga iniziare a prendere i colori', ui.ButtonSet.OK_CANCEL);
            if (output.getSelectedButton() == ui.Button.OK) {
                colorToHex(response.getResponseText(),output.getResponseText(),rowToStart.getResponseText());
            } else if (response.getSelectedButton() == ui.Button.CANCEL) {
                Logger.log('The user canceled the dialog.');
            } else {
                Logger.log('The user closed the dialog.');
            }
        }
        function hexToColor(colorColumn,hexColumn,rowToStart) {
            var sheet = SpreadsheetApp.getActiveSheet();
            var range = sheet.getDataRange();
            var values = range.getValues();
            for(r = rowToStart; r <= values.length; r++) {
                var row = values[r];
                range.getCell(r,hexColumn).setValue(range.getCell(r,colorColumn).getBackground());
                
            }
        }
        function hexToColorPrompt() {
            var ui = SpreadsheetApp.getUi();
            var response = ui.prompt('ColorManager', 'inserisci la colonna dove prendere i color', ui.ButtonSet.OK_CANCEL);
            var output = ui.prompt('ColorManager', 'inserisci la colonna dove inserire gli hex', ui.ButtonSet.OK_CANCEL);
            var rowToStart = ui.prompt('ColorManager', 'inserisci da quale riga iniziare a prendere i colori', ui.ButtonSet.OK_CANCEL);
            if (rowToStart.getSelectedButton() == ui.Button.OK) {
                hexToColor(response.getResponseText(),output.getResponseText(),rowToStart.getResponseText());
            } else if (response.getSelectedButton() == ui.Button.CANCEL) {
                Logger.log('The user canceled the dialog.');
            } else {
                Logger.log('The user closed the dialog.');
            }
        }
        function UtilityManagerMenu()
            {
                SpreadsheetApp.getUi().createMenu("UtilityManager")
                    .addItem("colorToHex", "hexToColorPrompt")
                    .addItem("hexToColor","colorToHexPrompt")
                    .addItem("imageWidth","imageWidthToPrompt")
                    .addItem("imageHeight","imageHeightToPrompt")
                    .addItem("opacityOnHex","hexWithOpacityPrompt")
                    .addToUi()
        }
        

```
