# snippets
Code snippets for reference

## AJAX

**Objective** Sending data via ajax to action in Yii controller/action, returned data is used to replace the whole form. Contains the syntax for more than 1D array

	$("#create_dispatch").click(function(e){
		var finals = {};
		//usually for dynamically created items (though in this case is more like already generated and just hiding it)
		//object creation for passing in as ajax data
		for (i = 0; i < 40; i++) {
			var field_op = $('#DispatchItem_create' + '_' + i + '_op');
			row = field_op.parents('tr');
			if (row.is(':visible')) {
				finals[i] ={
					'op':1, 
					'sort': i,
					'name': $('#DispatchItem_create' + '_' + i + '_name').val(),
					'description': $('#DispatchItem_create' + '_' + i + '_description').val(),
					'size': $('#DispatchItem_create' + '_' + i + '_size').val(),
					'count': $('#DispatchItem_create' + '_' + i + '_count').val(),
				};
			}
			else {
				finals[i] = {
					'op':0, 
					'sort': '',
					'name':'',
					'description':'',
					'size': '',
					'count': '',
				};
			}
		}

		//ajax object data, 3D array
		//_POST['Dispatch']['name'], _POST['DispatchItem']['create']['name']
    	var items={
    		'Dispatch':{
				name:  $('#Dispatch_name').val(), 
		    	fast_track: 1,
		    	contact_id: $("#Dispatch_contact_id").chosen().val(),
		    	attention: $('#Dispatch_attention').val(),
		    	authorized_by: $('#Dispatch_authorized_by').val()
	    	},
	    	'DispatchItem': {
	    		 'create':  finals  		
	    	},
    	};
 
		$.ajax({
			type: 'POST',
			url: 'index.php?r=dispatch/create',
			data: items,
			success:function(data){
				$("#dispatchFastTrack").html(data);
				//remember to call the function again if not the form can only be submitted once
				sendDispatchAjax();
				$(".dispatch_success").delay(3000).fadeOut("slow");
			},
			error: function(data) { // if error occured
				alert("Error occured.please try again");
				//alert(data);
			},

		});

	});

## UI / UX for Ajax 

### Warning Message (based on Bootstrap)
**Objective** Errors can be passed in and a error message (from Bootstrap) will be generated, comes with dismissable button, and will fade out after certain time)

	function createAlertPop(message, dest, error) {
		var alertState = 'alert-success';
		var button = '<button type="button" class="close" data-dismiss="alert" aria-hidden="true">×</button>';

		if (typeof error !== "undefined"){
			alertState = 'alert-danger';
		}

		var outputMsg = '<div class="alert alert-dismissable '+alertState+'">' + button + message + '</div>';

		$(outputMsg).hide().fadeIn('400').prependTo(dest).delay('5500').fadeOut(400, function(){
			$(this).remove();
		});

	}
