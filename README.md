

# DeepTrace

import javax.net.ssl.*;
import java.io.*;
import java.net.HttpURLConnection;
import java.net.URL;
import java.security.SecureRandom;
import java.security.cert.X509Certificate;
import java.util.Properties;
import org.json.JSONObject;

public class TokenUpdater {

    public static void main(String[] args) {
        try {
            // Disable SSL validation like Postman (for self-signed certs)
            disableSslVerification();

            // Fetch token from API
            String token = fetchToken();

            // Update token in properties file
            updatePropertiesFile("config.properties", "auth.token", token);

            System.out.println("✅ Token updated successfully: " + token);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private static String fetchToken() throws IOException {
        URL url = new URL("https://your-api-url.com/auth/token"); // Replace with your actual endpoint
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();

        conn.setRequestMethod("POST");
        conn.setDoOutput(true);

        // Set headers (as per your Postman example)
        conn.setRequestProperty("x-Context-Id", "test");
        conn.setRequestProperty("X-User-Id", "test");
        conn.setRequestProperty("X-Callers", "test");
        conn.setRequestProperty("X-Channel", "WEB");
        conn.setRequestProperty("Accept", "*/*");
        conn.setRequestProperty("Content-Type", "application/json; charset=UTF-8");

        // Set request body (adjust to your API's format)
        String jsonInput = "{ \"client_id\": \"abc\", \"client_secret\": \"xyz\" }";

        try (OutputStream os = conn.getOutputStream()) {
            byte[] input = jsonInput.getBytes("utf-8");
            os.write(input, 0, input.length);
        }

        int status = conn.getResponseCode();
        if (status != 200) {
            throw new RuntimeException("Failed to get token. HTTP error code: " + status);
        }

        StringBuilder response;
        try (BufferedReader br = new BufferedReader(
                new InputStreamReader(conn.getInputStream(), "utf-8"))) {
            response = new StringBuilder();
            String responseLine;
            while ((responseLine = br.readLine()) != null) {
                response.append(responseLine.trim());
            }
        }

        conn.disconnect();

        // Parse JSON response
        JSONObject json = new JSONObject(response.toString());
        return json.getString("access_token"); // adjust key if your token is under another field
    }

    private static void updatePropertiesFile(String filePath, String key, String newValue) throws IOException {
        Properties props = new Properties();
        try (InputStream input = new FileInputStream(filePath)) {
            props.load(input);
        }

        props.setProperty(key, newValue);

        try (OutputStream output = new FileOutputStream(filePath)) {
            props.store(output, null);
        }
    }

    // Disable SSL certificate validation (like Postman)
    private static void disableSslVerification() throws Exception {
        TrustManager[] trustAllCerts = new TrustManager[]{
            new X509TrustManager() {
                public void checkClientTrusted(X509Certificate[] certs, String authType) {}
                public void checkServerTrusted(X509Certificate[] certs, String authType) {}
                public X509Certificate[] getAcceptedIssuers() { return new X509Certificate[0]; }
            }
        };

        SSLContext sc = SSLContext.getInstance("TLS");
        sc.init(null, trustAllCerts, new SecureRandom());

        HttpsURLConnection.setDefaultSSLSocketFactory(sc.getSocketFactory());
        HttpsURLConnection.setDefaultHostnameVerifier((hostname, session) -> true);
    }
}
