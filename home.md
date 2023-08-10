---
layout: page
title:  "Overview"
categories: overview
author: Sandra
datatable: true

---

# Maturity Overview of DSL Ecosystems 

This table shows the information about all DSL's currently in the catalog:


<html>
<!-- js, html author: sandragreiner, jabier.martinez (partly) -->
<head>
<meta charset='UTF-8'>
<link rel="stylesheet" type="text/css"
	href="https://cdn.datatables.net/v/dt/jq-2.2.4/dt-1.10.15/datatables.min.css" />

<link rel="stylesheet" type="text/css"
	href="https://cdn.datatables.net/fixedheader/3.4.0/css/fixedHeader.dataTables.min.css" />
	
	
<link rel="stylesheet" type="text/css"
	href="https://cdn.datatables.net/1.13.5/css/jquery.dataTables.min.css" />
		<link rel="stylesheet" type="text/css"
	href="https://cdn.datatables.net/buttons/2.4.1/css/buttons.dataTables.min.css" />
	
<style type="text/css">
.dataTables_filter, .dataTables_info { display: none;}
tfoot {
	display: table-header-group;
}
input {
	width: 80px
}
.dataTables_wrapper.dt-buttons {
  float:none;  
  text-align:center;
}
</style>


<!-- Enables Sorting and Filtering -->
<script type="text/javascript"
	src="https://cdn.datatables.net/v/dt/jq-2.2.4/dt-1.10.15/datatables.min.js">
</script>
<!-- Export button -->
<script type="text/javascript"
	src="https://cdn.datatables.net/buttons/2.4.1/js/dataTables.buttons.min.js">
</script>
<script type="text/javascript"
	src="https://cdn.datatables.net/buttons/2.4.1/js/buttons.html5.min.js"></script>
<!-- Fixed header -->
<script type="text/javascript"
	src="https://cdn.datatables.net/fixedheader/3.4.0/js/dataTables.fixedHeader.min.js"></script>
	
<!-- Enable dynamic adjustment of romes -->
<script type="text/javascript"
	src="https://cdn.datatables.net/responsive/2.5.0/js/dataTables.responsive.min.js">
</script>


	
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
			// table entrys may be split among several pages
			paging : true,
			// allows for having two headers, first one for sorting, second one for filtering
			bSortCellsTop: true,
			// header rows are fixed
		    fixedHeader: true,
			scrollCollapse: true,
			scrollX: 3500,
			scrollY : true,
		    // allow for having an export button
	        dom: 'Bfrtip',
			responsive: false,
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
	        var col = table.column($(this).index());
	        $('input', this).on('keyup change', function () {
	            if (col.search() !== this.value) {
	                col.search(this.value).draw();
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

	<table id="grid" class="display" width="300%" cellspacing="20">
	
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
			

			<!-- condition doesn't work so far -->
			{% if {%row().index()%} == 2   %}
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

