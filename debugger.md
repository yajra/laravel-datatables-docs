# Debugging Mode

To enable debugging mode, just set ```APP_DEBUG=true``` and the package will include the queries and inputs used when processing the table.

> IMPORTANT: Please make sure that APP_DEBUG is set to false when your app is on production.

## Example Response
```json
{
	"draw": 1,
	"recordsTotal": 10,
	"recordsFiltered": 3,
	"data": [{
		"id": 414,
		"name": "Abel Cole",
		"email": "jklein@block.com",
		"created_at": "2016-07-31 23:26:10",
		"updated_at": "2016-07-31 23:26:10",
		"deleted_at": null,
		"superior_id": 0
	}, {
		"id": 267,
		"name": "Addie Satterfield",
		"email": "cassin.eva@pouros.info",
		"created_at": "2016-07-31 23:26:00",
		"updated_at": "2016-07-31 23:26:00",
		"deleted_at": null,
		"superior_id": 0
	}, {
		"id": 120,
		"name": "Adeline Mayert",
		"email": "rice.elian@abshire.com",
		"created_at": "2016-07-31 23:25:50",
		"updated_at": "2016-07-31 23:25:50",
		"deleted_at": null,
		"superior_id": 0
	}],
	"queries": [{
		"query": "select count(*) as aggregate from (select '1' as `row_count` from `users` where `users`.`deleted_at` is null and `users`.`deleted_at` is null) count_row_table",
		"bindings": [],
		"time": 1.84
	}, {
		"query": "select * from `users` where `users`.`deleted_at` is null order by `name` asc limit 10 offset 0",
		"bindings": [],
		"time": 1.8
	}],
	"input": {
		"draw": "1",
		"columns": [{
			"data": "id",
			"name": "",
			"searchable": "true",
			"orderable": "true",
			"search": {
				"value": "",
				"regex": "false"
			}
		}, {
			"data": "name",
			"name": "",
			"searchable": "true",
			"orderable": "true",
			"search": {
				"value": "",
				"regex": "false"
			}
		}, {
			"data": "email",
			"name": "",
			"searchable": "true",
			"orderable": "true",
			"search": {
				"value": "",
				"regex": "false"
			}
		}, {
			"data": "created_at",
			"name": "",
			"searchable": "true",
			"orderable": "true",
			"search": {
				"value": "",
				"regex": "false"
			}
		}, {
			"data": "updated_at",
			"name": "",
			"searchable": "true",
			"orderable": "true",
			"search": {
				"value": "",
				"regex": "false"
			}
		}],
		"order": [{
			"column": "1",
			"dir": "asc"
		}],
		"start": "0",
		"length": "10",
		"search": {
			"value": "",
			"regex": "false"
		},
		"_": "1479295888286"
	}
}
```