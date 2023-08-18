
```java
	@Override  
	public List<PurchaseOrderDto> getProductsInfo(Long purchaseOrderId) {  
		String jpql = """  
			SELECT new maktab.ecommerce.PurchaseOrderDto(p.name, op.qty)  
		FROM OrderProduct op  
		JOIN op.product p  
		WHERE op.purchaseOrder.id = :purchaseOrderId  
	""";  
	return em.createQuery(jpql, PurchaseOrderDto.class)  
		.setParameter("purchaseOrderId", purchaseOrderId)  
		.getResultList();  
	}
```