package server;

import java.io.ByteArrayInputStream;
import java.io.InputStream;
import java.io.StringWriter;

import javax.xml.soap.*;
import javax.xml.transform.*;
import javax.xml.transform.stream.*;

public class ConnToWS {

    /**
     * Starting point for the SAAJ - SOAP Client Testing
     */
    public String  connToWS(String req) {
        try {
            // Create SOAP Connection
            SOAPConnectionFactory soapConnectionFactory = SOAPConnectionFactory.newInstance();
            SOAPConnection soapConnection = soapConnectionFactory.createConnection();

            // Send SOAP Message to SOAP Server
            String url = "http://10.246.246.31:7800/BackEndService.asmx";
            SOAPMessage soapResponse = soapConnection.call(createSOAPRequest(req), url);
            soapConnection.close();
            // Process the SOAP Response and return it to call back 
            
           return printSOAPResponse(soapResponse);           
        } catch (Exception e) {
            System.err.println("Error occurred while sending SOAP Request to Server");
            e.printStackTrace();
            return "Broker No Disponible.";
        }
    }

    private static SOAPMessage createSOAPRequest(String req) throws Exception {
    	String send = req; 
    	InputStream is = new ByteArrayInputStream(send.getBytes());
    	SOAPMessage request = MessageFactory.newInstance().createMessage(null, is);
    	
    	 SOAPMessage soapMessage = request;
        
        /* Print the request message */
        System.out.print("Request SOAP Message = ");
        soapMessage.writeTo(System.out);
        System.out.println();

        return soapMessage;
    }

    /**
     * Method used to print the SOAP Response
     */
    private static String printSOAPResponse(SOAPMessage soapResponse) throws Exception {
             
        StringWriter writer = new StringWriter();
        StreamResult result = new StreamResult(writer);
        
        TransformerFactory transformerFactory = TransformerFactory.newInstance();
        Transformer transformer = transformerFactory.newTransformer();
        
        Source sourceContent = soapResponse.getSOAPPart().getContent();
        
        transformer.transform(sourceContent, result);
        
        String rs = writer.toString(); 
       // System.out.println("\nMagia: "+rs);
        
        return rs;
    }

}









