_I need to update this with the information from the enhancements I did with Apache POI to make GREAT SPREADSHEETS_

# Introduction #

data:uri is not fully supported in browsers.  Were it supported the same code would work on IE as it does on Firefox,  and you could have full control over the filename (if it is a CSV export you are doing).  From the examples you can find,  mostly on the ExtJS forums,  the code you have to add is complex and doesn't do what you need it to do.

Therefore,  small server-side echo functions are required to help your AJAX page to do CSV export download files (not to mention PDF or Word or other formats for whatever data the user filters on the ajax page).

The purpose is to do all the real work on the browser,  and only echo the dataset in various formats from the server (do not duplicate so much data query facilities on the server,  duplicating what is done with Trimpath Query,  for example).


# Details #

To begin with the problem is partitioned into two parts,  one where the customer wishes to download the entire database,  this is a standard fixed call to a server-side getAll function (not through the echo mechanism).

The second part is the echo part,  a different link to download the search results that are sitting in a result set from a trimpath query.

## Part 1:  User wants entire thing. ##
Bypass echo with standard server get CSV file.  I include this here just for a complete context, which might be useful to some people.  It's a Spring Portlet controller method.  Java, yes,  sorry, I can't help it,  gotta eat.  The domain is Records Management -- the lifeblood of any organization.
```
	/**
	 * Retrieve entire Record Retention Schedules and return in CSV rather than JSON.
	 */
	@RequestMapping(params = "do=retrieveRecRetSchedDataCSV")
	public void retrieveRecRetSchedDataCSV(ResourceRequest request, ResourceResponse response) {

		String dataSetName = "RecordRetentionSchedules";
		String filename = dataSetName+".csv";
		String output = "\"test,  ok\", \"test\"";

		RecRetSchedSettings settings = loadSettings();

		RecRetSchedData data = new RecRetSchedData();

	    retrieveRecRetSchedItems( data, null );
	    
		String newListingData = 	    		  
			"SCHEDULE ID,"+
			"COST_CENTER_CODE," +
			"DEPT_CODE,"+
			"DEPT_NAME,"+
			"DEPT_OAS_FLAG,"+
			"DEPT_LOCN_ID,"+
			"DEPT_ACTIVE_FLAG,"+
			"RECORD_CODE,"+
			"RECORD_TITLE,"+
			"RECORD_DESCR,"+
			"DEPT_RETENTION_DESCR,"+
			"TOTAL_RETENTION_DESCR,"+
			"EVENT_BASED_FLAG,"+  
			"DEPT_RETENTION_QTY,"+
			"TOTAL_RETENTION_QTY,"+
			"DEPT_RETENTION_UNIT,"+
			"TOTAL_RETENTION_UNIT,"+
			"OAS_FLAG,"+
			"GENERAL_FLAG,"+
			"ACTIVE_FLAG\n";
		
		int rowCounter = 0;
		RecordCode currentSchedItem = null;
		
	    try {

			for( RecordCode schedItem : data.recRetSchedItems ){
	    		  			  
			  if ( rowCounter++ > 0 ) { newListingData += "\n"; }
			  
			  currentSchedItem = schedItem;

			  Department blankDept = new Department();
			  blankDept.setDeptName("");
			  blankDept.setActiveFlag("");
			  blankDept.setCostcode("");
			  blankDept.setDeptCode("");
			  blankDept.setLocnIdDefault(new BigDecimal(0));
			  blankDept.setOasFlag("");
			  
			  newListingData +=  
				  schedItem.getId()+", " +
				  nv2(schedItem.getDept(), blankDept).getCostcode()+", " +
				  nv2(schedItem.getDept(), blankDept).getDeptCode()+", " +
				  nv2(schedItem.getDept(), blankDept).getDeptName().replace( "\"", "'").replace( "<", " sym-lt ").replace( ">", " sym-gt ").replace( "\n", " ").replace( ",", " ")+", " +
				  nv2(schedItem.getDept(), blankDept).getOasFlag()+", " +
				  nv2(schedItem.getDept(), blankDept).getLocnIdDefault()+", " +
				  nv2(schedItem.getDept(), blankDept).getActiveFlag()+", " +
				  schedItem.getRecordCode()+", " +
				  nv2(schedItem.getRecordTitle(),"").replace( "\"", "'").replace( "<", " sym-lt ").replace( ">", " sym-gt ").replace( "\n", " ").replace( ",", " ")+", " +
				  nv2(schedItem.getRecordDescr(),"").replace( "\"", "'").replace( "<", " sym-lt ").replace( ">", " sym-gt ").replace( "\n", " ").replace( ",", " ")+", " +		  
				  nv2(schedItem.getDeptRetentionDescr(),"").replace( "\"", "'").replace( "<", " sym-lt ").replace( ">", " sym-gt ").replace( "\n", " ").replace( ",", " ")+", " +
				  nv2(schedItem.getTotalRetentionDescr(),"").replace( "\"", "'").replace( "<", " sym-lt ").replace( ">", " sym-gt ").replace( "\n", " ").replace( ",", " ")+", " +
				  schedItem.getEventBasedFlag()+", " +		  
				  schedItem.getDeptRetentionQty()+", " +
				  schedItem.getTotalRetentionQty()+", " +
				  schedItem.getDeptRetentionUnit()+", " +
				  schedItem.getTotalRetentionUnit()+", " +
				  schedItem.getOasFlag()+", " +
				  schedItem.getGeneralFlag()+", " +
				  schedItem.getActiveFlag()+" ";
			  
			}
			
			
	    } catch (Exception e) {
			logger.error("RRS.retrieveRecRetSchedData(): failed to parse list at index "+rowCounter+" RecordCode: "+currentSchedItem+" Exception: "+e.toString());
	    }
	    
		newListingData += "\n";
		
		output = newListingData;
		
		response.setContentType("text/csv");
		Long size = null;
		if (size != null) {
			response.setProperty("Content-Disposition", "attachment; filename=" + filename + "; size=" + size);
		} else {
			response.setProperty("Content-Disposition", "attachment; filename=" + filename);
		}

		try {
			PrintWriter pw;
			pw = response.getWriter();
			if (output != null) {
				pw.print(output);
			} else {
				pw.print("output is null");
			}
			pw.flush();

		} catch (IOException e) {
			e.printStackTrace();
		}
    
	}
```

Nv2() demonstrates 2005 Java skills (rather than 1999 Java skills),  through the use of Generics:

```
	private <T> T nv2(T a, T b) {
		return (a == null)?b:a;
	}
```

End of partition 1 of the problem.  Partition one is the user simply wants all the data in whatever form without any help from the Ajax interface.

Now the interesting part.

## Part 2. Trimpath Ajax Echo Helper ##

In a Spring MVC controller context,  at the bottom of your view.jsp you have a launch pad:
```
<form name="ajax_echo_launcher" id="ajax_echo_launcher" action="${ajaxEchoServiceCSVURL}" method="post" style="display:none;"> 
<textarea name="ajax_echo_area" id="ajax_echo_area" rows="15" cols="80"></textarea>
</form>
```

The ajax\_echo\_area is populated with CSV content from a recent query:
```
	prepareAndSendEcho: function() {
		var the_col_names = [];
		for(var name in policy_viewer_records_columnDefs.recordcodes) {
			if(! (policy_viewer_records_columnDefs.recordcodes[name] instanceof Function)) {
				the_col_names.push(name);
			}
		}
		var echo_struct = { dataset_name: this.currentListingLabel, col_headers: the_col_names, data: this.currentListing };
		jQuery("#ajax_echo_area").html(YAHOO.lang.JSON.stringify(echo_struct));
		jQuery("#ajax_echo_launcher").get(0).submit();

	}
```

In the above,  this.currentListing is the return data from a recent trimpath query.  policy\_viewer\_records\_columnDefs.recordcodes is the Trimpath Query schema from which I grab all column names for the header:
```
   var policy_viewer_records_columnDefs = {
		   recordcodes  : 
     	   { 
	         id          : { type: "String" },
	         cost_center_code     : { type: "String" },
	         dept_code     : { type: "String" },
	         dept_name:  { type: "String" },
	         dept_oas_flag          : { type: "String" },
	         dept_locn_id     : { type: "String" },
	         dept_active_flag          : { type: "String" },
	         record_code: { type: "String" },
	         record_title: { type: "String" },
	         record_descr: { type: "String" },
	         dept_retention_descr: { type: "String" },
	         total_retention_descr: { type: "String" },
             event_based_flag: { type: "String" },
             dept_retention_qty: { type: "String" },
             total_retention_qty: { type: "String" },
             dept_retention_unit: { type: "String" },
             total_retention_unit: { type: "String" },
             oas_flag: { type: "String" },
             general_flag: { type: "String" },
             active_flag: { type: "String" }
		  
            }
        };    
```

currentListingLabel is simply a string put together to be the filename,  which is just a representation of whatever optionVals and keywordVals the user was querying on:
```
		 this.currentListingLabel = "RecordRetentionSchedules"+optionVal+keywordsVal.replace(/[^a-zA-Z0-9]/g, '');
```

Ok,  that does it for the javascript side.  The form gets launched at the server with a JSON struct representing a CSV to get spit back as an actual CSV download with the text/csv mime header setting and the filename to be saved as.

The server-side piece that responds is here,  in a Spring Portlet context:
```

	/**
	 * Ajax Echo Service -- Take a JSON in a textarea POST input and echo back a CSV.
	 * This should eventually be a separate servlet in a separate WAR file.
	 * 
	 * This is an ajax utility because the data: uri doesn't completely support
	 * what you want to do in ajax (and no support at all in IE).  There would
	 * be a variety of ajax echo services,  such as PDF etc.  There is no support
	 * for naming the file in data: uri for example,  you just get gooblygook
	 * for a filename.   Need serverside helpers for various tasks.
	 * 
	 * Example input JSON:
	 * { 
	 *     "col_headers": ["id","cost_center_code","dept_code","dept_name",....],
	 *     "data": [{"active_flag":"I","cost_center_code":"005460","dept_active_flag":"T","dept_code":"OIT",...}],
	 *     "dataset_name":"RecordRetentionScheduleshelpdesk"
	 * }
	 * 
	 * the col headers determines the order and selection of data values.
         *
	 *  This function uses the CSV utilities from http://ostermiller.org/utils/.
	 */
	
	@RequestMapping(params = "do=ajaxEchoServiceCSV")
	public void ajaxEchoServiceCSV(ResourceRequest request, ResourceResponse response) {

		String jsonString = request.getParameter("ajax_echo_area");

		JSONObject echoObject = null;
	    try {
	    	echoObject = new JSONObject(jsonString); 
	    } catch (Exception e) {
			logger.error("RRS.ajaxEchoServiceCSV(): failed to JSONify listing string rep: " + e + jsonString);
	    }
	    
		String dataSetName = null;
		JSONArray csvHeaders = null;
		JSONArray csvData = null;
		try {
			dataSetName = echoObject.getString("dataset_name");
			csvHeaders = echoObject.getJSONArray("col_headers");
			csvData = echoObject.getJSONArray("data");
		} catch (Exception e1) {
			logger.error("RRS.ajaxEchoServiceCSV(): incorrect data structure: " + e1);
		}
		
		String lineOne [] = new String[csvHeaders.length()];
		
		for (int i = 0; i < csvHeaders.length(); i++) {
			try {
				lineOne[ i ] = csvHeaders.getString(i).toUpperCase();
			} catch (Exception e) {
				logger.error("RRS.ajaxEchoServiceCSV(): error getting lineOne: " + e);
			}
		}
		
		//fix SYLK oddity from Microsoft
		if ( lineOne[0].equals("ID") ) {
			lineOne[0] = "RRSID";
		}
		
		String dataLines [][] = new String[csvData.length()][csvHeaders.length()];
		try {
			for (int j = 0; j < csvData.length(); j++) {
				JSONObject currentDataRecord = csvData.getJSONObject(j);
				for (int i = 0; i < csvHeaders.length(); i++) {
						dataLines[ j ][ i ] = currentDataRecord.getString(csvHeaders.getString(i));
				}
			}
		} catch (Exception e) {
			logger.error("RRS.ajaxEchoServiceCSV(): error getting dataLines: " + e);
		}
		
		String filename = dataSetName+".csv";

		response.setContentType("text/csv");
		Long size = null;
		if (size != null) {
			response.setProperty("Content-Disposition", "attachment; filename=" + filename + "; size=" + size);
		} else {
			response.setProperty("Content-Disposition", "attachment; filename=" + filename);
		}

		try {
			PrintWriter pw;
			pw = response.getWriter();
			ExcelCSVPrinter exclcsv = new ExcelCSVPrinter(pw);
            exclcsv.println(lineOne);
            exclcsv.println(dataLines);
			pw.flush();
		} catch (IOException e) {
			e.printStackTrace();
		}
    
	}

```

This function is generic enough to echo back any result set with any columns (all that work is done on the browser).

The ostermiller utils are extremely useful for many things in addition to CSV,  but will not distribute here due to the GPL.  If they suddenly disappear off the web,  email me and I'll forward you a copy.  --kucerarichard