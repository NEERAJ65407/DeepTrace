

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
            // 1. Disable SSL certificate verification (for testing)
            disableSslVerification();

            // 2. Fetch token from API
            String token = fetchToken();

            // 3. Update the 'clolite' key in your .properties file
            updatePropertiesFile("config.properties", "clolite", token);

            System.out.println("✅ Token fetched and updated successfully.");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private static String fetchToken() throws IOException {
        URL url = new URL("https://your-api-url.com/auth/token"); // Replace with your actual API
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();

        conn.setRequestMethod("POST");
        conn.setDoOutput(true);

        // 🟨 Headers
        conn.setRequestProperty("x-Context-Id", "test");
        conn.setRequestProperty("X-User-Id", "test");
        conn.setRequestProperty("X-Callers", "test");
        conn.setRequestProperty("X-Channel", "WEB");
        conn.setRequestProperty("Accept", "*/*");
        conn.setRequestProperty("Content-Type", "application/json; charset=UTF-8");

        // 🟨 Body (adjust as per your API)
        String jsonInput = "{ \"client_id\": \"abc\", \"client_secret\": \"xyz\" }";
        try (OutputStream os = conn.getOutputStream()) {
            os.write(jsonInput.getBytes("utf-8"));
        }

        int status = conn.getResponseCode();
        if (status != 200 && status != 201) {
            throw new RuntimeException("Failed to get token. HTTP error code: " + status);
        }

        StringBuilder response = new StringBuilder();
        try (BufferedReader br = new BufferedReader(
                new InputStreamReader(conn.getInputStream(), "utf-8"))) {
            String responseLine;
            while ((responseLine = br.readLine()) != null) {
                response.append(responseLine.trim());
            }
        }

        System.out.println("🔍 API Response:\n" + response.toString());

        conn.disconnect();

        // 🟨 Extract token from response (adjust based on your actual JSON)
        JSONObject json = new JSONObject(response.toString());
        return json.getString("access_token"); // Modify this if the key is different or nested
    }

    private static void updatePropertiesFile(String filePath, String key, String newValue) throws IOException {
        Properties props = new Properties();

        // 🔁 Load existing properties
        try (InputStream input = new FileInputStream(filePath)) {
            props.load(input);
        }

        // ✏️ Replace specific key
        props.setProperty(key, newValue);

        // 💾 Save updated properties
        try (OutputStream output = new FileOutputStream(filePath)) {
            props.store(output, null);
        }

        System.out.println("✅ Updated '" + key + "' in " + filePath);
    }

    private static void disableSslVerification() throws Exception {
        TrustManager[] trustAllCerts = new TrustManager[] {
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
