## PII Data Privacy

<div style="text-align: center;"><img src="../guide/assets/images/pii-graphic.png"></div>


### Acceso a API Users:
?> Para esta APP no sería necesaria una migración ya que la llamada actual a Users se realiza mediante la firma de usuario público, cuya respuesta no esta alcanzada por la migración a VAULT.


### Storage:

- DB: No aplica

- KVS: No aplica

    - returns-context-cache-kvs (key: buyer_returns_order_[ORDER_ID]) // Linkeable
```json 
    {
    	"resource_id": [ORDER_ID], // Linkeable
    	"buyer_id": [USER_ID] // Linkeable
    	"seller_id": [USER_ID] // Linkeable
    	"return": {
    		"id": "[RETURN_ID]", // Linkeable,
    		"claim": [CLAIM_ID] // Linkeable
            ...
    	},
    	"shipment": {
    		"id": [SHIPMENT_ID], // Linkeable
    		...
    	},	
    	"orders": [{
    		"id": [ORDER_ID], // Linkeable
    		"payment": [{
    			"id": [PAYMENT_ID], // Linkeable
    			...
    		}]
            ... 
    	}],
    	...
    }
```
    - prod-context-cache-kvs (key: context_order_[ORDER_ID]) // Linkeable
```json 
    {
        "order": "order_[ORDER_ID]", // Linkeable
        "claim": "claim_[CLAIM_ID]" // Linkeable
    }
```
    - prod-entity-cache-kvs (key: claim_[CLAIM_ID]) // Linkeable
```json 
    {
        "id": [CLAIM_ID] // Linkeable
        ...
        "players": [
            {
                "user_id": [USER_ID] // Linkeable
                ...
            },
            {
                "user_id": [USER_ID] // Linkeable
                ...
            }
        ],
        ...
    }
```
    
- Caches: No aplica

- Object-storage: No aplica

- DS: No aplica


### Api Endpoints:

* Request: GET `/post-purchase/claims/messages?client.id=$APP_ID&caller.id=$USER_ID&resource_id=$PACK_ID|$ORDER_ID&resource_type=$RESOURCE_TYPE`

* Params:
|     Key      |  Value  | Linkable |
| :----------: | :-----: | :------: |
|  $APP_ID | Long  |    ✖️     |
|  $USER_ID | Long  |    ✔️     |
|  $PACK_ID | Long  |    ✔️     |
|  $ORDER_ID | Long  |    ✔️     |
|  $RESOURCE_TYPE | String  |    ✖️     |

* Response: (No necesariamente todos los bloques)
```json
{
	"resource_id": [ORDER_ID], // Linkeable
	"status": {
		"actions": [{
			"content": "[URL_MEDIATIONS]/claims/[CLAIM_ID]?flow=purchases&urlBack=[URL_MYACCOUNT]/my_purchases/[ORDER_ID]/status" // Linkeables
          	...
		}]
      	...
	},
	"secondary": {
		"actions": [{
			"content": "[URL_MEDIATIONS]/claims/[CLAIM_ID]?flow=purchases&urlBack=[URL_MYACCOUNT]/my_purchases/[ORDER_ID]/status" // Linkeables
          	...
		}]
      	...
	},
	"metadata": {
		"shipping_id": [SHIPPING_ID], // Linkeable
		...
	},
    ...
}
```

---

* Request: GET `/post-purchase/returns/messages?client.id=$APP_ID&caller.id=$USER_ID&resource_id=$PACK_ID|$ORDER_ID&resource_type=$RESOURCE_TYPE&use_kvs=$USE_KVS`

* Params:
|     Key      |  Value  | Linkable |
| :----------: | :-----: | :------: |
|  $APP_ID | Long  |    ✖️     |
|  $USER_ID | Long  |    ✔️     |
|  $PACK_ID | Long  |    ✔️     |
|  $ORDER_ID | Long  |    ✔️     |
|  $RESOURCE_TYPE | String  |    ✖️     |
|  $USE_KVS | Boolean  |    ✖️     |

* Response: (No necesariamente todos los bloques)
```json
{
	"resource_id": [ORDER_ID], // Linkeable
	"status": {
		"modals": [{
			"actions": { 
              "content": "[RETURN_ID]", // Linkeable
              ... 
             } 
          	...
		}]
      	...
	}
    ...
}
```

---

* Request: GET `/post-purchase/kyc/messages?client.id=$APP_ID&caller.id=$USER_ID&resource_id=$PACK_ID|$ORDER_ID&resource_type=$RESOURCE_TYPE&site_id=$SITE_ID`

* Params:
|     Key      |  Value  | Linkable |
| :----------: | :-----: | :------: |
|  $APP_ID | Long  |    ✖️     |
|  $USER_ID | Long  |    ✔️     |
|  $PACK_ID | Long  |    ✔️     |
|  $ORDER_ID | Long  |    ✔️     |
|  $RESOURCE_TYPE | String  |    ✖️     |
|  $SITE_ID | String  |    ✖️     |

* Response: (No necesariamente todos los bloques)
```json
{
	"resource_id": [ORDER_ID], // Linkeable
	"status": {
		"actions": [{
			"content": "[URL_MEDIATIONS]/claims/[CLAIM_ID]?flow=purchases&urlBack=[URL_MYACCOUNT]/my_purchases/[ORDER_ID]/status" // Linkeables
          	...
		}]
      	...
	},
	"secondary": {
		"actions": [{
			"content": "[URL_MEDIATIONS]/claims/[CLAIM_ID]?flow=purchases&urlBack=[URL_MYACCOUNT]/my_purchases/[ORDER_ID]/status" // Linkeables
          	...
		}]
      	...
	},
	"metadata": {
		"shipping_id": [SHIPPING_ID], // Linkeable
		...
	},
    ...
}
```

---

* Request: POST `/consumers/claims`

* Body:
```json
{
	"msg": {
		"id": [CLAIM_ID] // Linkeable
		"resource_id": [ORDER_ID], // Linkeable
		...
    }
}
```

* Response: (Mismo Body q recibe)

---

* Request: POST `/consumers/returns`

* Body:
```json
{
	"msg": {		
		"claim": [CLAIM_ID] // Linkeable
		"id": [RETURN_ID] // Linkeable
		"seller_id": [USER_ID] // Linkeable
        "buyer_id": [USER_ID] // Linkeable
		"shipment": [SHIPPING_ID], // Linkeable
		...
	}
}
```

* Response: (Mismo Body q recibe)


### BigQueue Consumers:

- prod-claims-consumer:
> Ver Endpoint `/consumers/claims`

- prod-return-consumer:
> Ver Endpoint `/consumers/returns`