

# DeepTrace

import java.io.*;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.Properties;
import org.json.JSONObject;

public class TokenUpdater {

    public static void main(String[] args) {
        try {
            // Step 1: Make POST request to get token
            String token = fetchToken();

            // Step 2: Update properties file
            updatePropertiesFile("config.properties", "auth.token", token);

            System.out.println("Token updated successfully: " + token);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private static String fetchToken() throws IOException {
        URL url = new URL("https://your-api-url.com/auth/token"); // Change to your URL
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();

        conn.setRequestMethod("POST");
        conn.setRequestProperty("Content-Type", "application/json");
        conn.setDoOutput(true);

        String jsonInput = "{\"username\": \"your_username\", \"password\": \"your_password\"}";
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

        // Extract token from JSON
        JSONObject json = new JSONObject(response.toString());
        return json.getString("access_token"); // adjust key if needed
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
}
