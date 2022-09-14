# esame


SELECT  t1.AlbumId , t2.AlbumId 
FROM playlisttrack p1,track t1,track t2 ,playlisttrack p2
WHERE t1.TrackId=p1.TrackId 
AND t2.TrackId=p2.TrackId
AND t1.TrackId > t2.TrackId
AND p1.PlaylistId=p2.PlaylistId
AND t1.AlbumId IN (SELECT tr1.AlbumId 
				FROM track tr1,album alb1
				WHERE alb1.AlbumId=tr1.AlbumId
				GROUP BY tr1.AlbumId
				HAVING SUM(tr1.Milliseconds)/1000> (120*60))
AND t2.AlbumId IN (SELECT tr2.AlbumId 
				FROM track tr2,album alb2
				WHERE alb2.AlbumId=tr2.AlbumId
				GROUP BY tr2.AlbumId
				HAVING SUM(tr2.Milliseconds)/1000> (120*60))
GROUP BY t1.AlbumId, t2.AlbumId


public int getComponenteConnessa(ArtObject vertice) {
		Set<ArtObject> visitati = new HashSet<>();
		
		
		DepthFirstIterator<ArtObject,DefaultWeightedEdge> it = 
				new DepthFirstIterator<ArtObject, DefaultWeightedEdge>(this.grafo, vertice);
		while (it.hasNext()) {
			visitati.add(it.next());
		}
		
		return visitati.size();
		
	}
  
  
  
  @FXML
    void doCalcolaComponenteConnessa(ActionEvent event) {
    	txtResult.clear();
    	int objectId;
    	try {
    		objectId = Integer.parseInt(txtObjectId.getText());
    	} catch (NumberFormatException e) {
    		txtResult.appendText("Devi inserire un codice numerico!");
    		return;
    	}
    	
    	ArtObject vertice = this.model.getObject(objectId);
    	
    	if(vertice == null) {
    		txtResult.appendText("Oggetto inesistente!");
    		return;
    	}
    	
    	int size = this.model.getComponenteConnessa(vertice);
    	txtResult.appendText("DIMENSIONE COMPONENTE CONNESSA: " + size);
