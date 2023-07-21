---
layout: page
title:  "Overview New"
categories: overview
author: Sandra
datatable: true

---

# Maturity Overview of DSL Ecosystems 

This table shows the information about all DSL's currently in the catalog:


<html>
<!-- js, html author: jabier.martinez -->
<head>
<meta charset='UTF-8'>
<link rel="stylesheet" type="text/css"
	href="https://cdn.datatables.net/v/dt/jq-2.2.4/dt-1.10.15/datatables.min.css" />

<link rel="stylesheet" type="text/css"
	href="https://cdn.datatables.net/fixedheader/3.1.2/css/fixedHeader.dataTables.min.css" />
	
<link rel="stylesheet" type="text/css"
	href="https://cdn.datatables.net/1.10.16/css/jquery.dataTables.min.css" />
		<link rel="stylesheet" type="text/css"
	href="https://cdn.datatables.net/buttons/1.4.1/css/buttons.dataTables.min.css" />
	
<!--link rel="stylesheet" type="text/css" href="assets/css/datatable"-->

<style type="text/css">
.dataTables_filter, .dataTables_info { display: none;}
tfoot {
	display: table-header-group;
}
input {
	width: 100px
}
.dataTables_wrapper.dt-buttons {
  float:none;  
  text-align:center;
}
</style>

<script type="text/javascript"
	src="https://cdn.datatables.net/v/dt/jq-2.2.4/dt-1.10.15/datatables.min.js">
</script>

<!-- Fixed header -->
<script type="text/javascript"
	src="https://cdn.datatables.net/fixedheader/3.1.2/js/dataTables.fixedHeader.min.js"></script>
<!-- Export buttons -->
<script type="text/javascript"
	src="https://cdn.datatables.net/buttons/1.4.1/js/dataTables.buttons.min.js">
</script>
<script type="text/javascript"
	src="https://cdn.datatables.net/buttons/1.4.1/js/buttons.flash.min.js"></script>
<script type="text/javascript"
	src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.1.3/jszip.min.js"></script>

<script type="text/javascript"
	src="https://cdn.datatables.net/buttons/1.4.1/js/buttons.html5.min.js"></script>
	
<script type="text/javascript" class="init">

	function countNumberOfCaseStudies() {
		// minus 2 because two <tr> represent the header and the search function
    	return (document.getElementsByTagName('tr').length - 2);
	}

	$(document).ready(function() {
		
		document.getElementById("insertCount").innerHTML = countNumberOfCaseStudies();

 		$('#grid thead th.input-filter').each(function() {
 			var title = $(this).text();
 			$(this).html('<input type="text" placeholder="Find" />');
 		});

 		function isURL(inputString) {
			return inputString.indexOf('http') == 0;
		}
    	
		
		function fileNameExport(){
 			var dateObj = new Date();

        	var year = dateObj.getUTCFullYear();
        	var month = dateObj.getUTCMonth();
        	var day = dateObj.getUTCDate();

        	return year +""+ month +""+ day + '_DSLMaturityLevels';
 		}

		var table = $('#grid').DataTable({
			// this allow to have two headers where the second one is used to filter and the first one to sort
			bSortCellsTop: true,
			// no pagination
			paging : true,
			// freeze the header rows
		    fixedHeader: true,
		    // export buttons
	        dom: 'Bfrtip',
	        buttons: [
	        	{
	                extend: 'csv',
	                filename: fileNameExport(),
	                exportOptions: {
              			stripHtml: false
            		}
	            }
	        ]
		});

		$.each($('.input-filter', table.table().header()), function () {
	        var column = table.column($(this).index());
	        $('input', this).on('keyup change', function () {
	            if (column.search() !== this.value) {
	                column.search(this.value).draw();
	            }
	        });
		});
	});
</script>
</head>

<body>
	<div  style="text-align:left">
		<b>How to use:</b> Filter and sort the columns to find the DSLs that you are interested in.<br/>
		<b>Current number of included case studies:</b> <i id="insertCount"></i><br/>
	</div>

	<table id="grid" class="display" width="300%" cellspacing="0">

	
	<thead>
	{% for row in site.data.overview %}
		{% if forloop.first %}
			<tr>
			{% for pair in row %}
				<th style="background-color: Gray;" align="center;">
				{{ pair[0] }}
				</th>
			{% endfor %}
			</tr>
			<tr class="userFilters">
			{% for pair in row %}
				<th class="input-filter">{{ pair[0] }}</th>
			{% endfor %}
			</tr>
   	 	{% endif %}
	{% endfor %}
	</thead>

	<tbody>
	{% for row in site.data.overview %}
		<tr>
		{% for pair in row %}
		<th style="background-color: LightGray;">
			
			<!-- {% if isURL(pair[1]); %}
			<a href="{{pair[1]}}"> {{pair[1]}}</a>
			{% endif %} -->

			<!-- condition doesn't work so far -->
			{% if x.indexOf('http') >= 0 %}
			<a href="{{pair[1]}}"> {{pair[1]}}</a>
			{% else %}
			{{pair[1]}}
			{% endif %}
		</th>
		{% endfor %}
		</tr>
	{% endfor %}

	{% for row in site.data.overview_addOns %}
		<tr>
		{% for pair in row %}
		<th style="background-color: LightGray;">
			{{pair[1]}}
		</th>
		{% endfor %}
		</tr>
	{% endfor %}
    </tbody>
	</table>
</body>
</html>

