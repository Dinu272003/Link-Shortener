import java.util.HashMap;
import java.util.Random;

public class LinkShortener {
    private static final String CHAR_SET = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    private static final int SHORT_URL_LENGTH = 6;
    private HashMap<String, String> shortToLongUrlMap;
    private HashMap<String, String> longToShortUrlMap;
    private Random random;

    public LinkShortener() {
        shortToLongUrlMap = new HashMap<>();
        longToShortUrlMap = new HashMap<>();
        random = new Random();
    }

    // Method to generate a short URL for a given long URL
    public String shortenUrl(String longUrl) {
        if (longToShortUrlMap.containsKey(longUrl)) {
            return longToShortUrlMap.get(longUrl);
        }

        String shortUrl;
        do {
            shortUrl = generateShortUrl();
        } while (shortToLongUrlMap.containsKey(shortUrl));

        shortToLongUrlMap.put(shortUrl, longUrl);
        longToShortUrlMap.put(longUrl, shortUrl);
        return shortUrl;
    }

    // Method to expand a short URL to its original long URL
    public String expandUrl(String shortUrl) {
        if (!shortToLongUrlMap.containsKey(shortUrl)) {
            return "Error: Short URL does not exist!";
        }
        return shortToLongUrlMap.get(shortUrl);
    }

    // Helper method to generate a unique short URL identifier
    private String generateShortUrl() {
        StringBuilder shortUrl = new StringBuilder(SHORT_URL_LENGTH);
        for (int i = 0; i < SHORT_URL_LENGTH; i++) {
            int index = random.nextInt(CHAR_SET.length());
            shortUrl.append(CHAR_SET.charAt(index));
        }
        return shortUrl.toString();
    }

    // Test functionality
    public static void main(String[] args) {
        LinkShortener linkShortener = new LinkShortener();

        String longUrl1 = "https://www.example.com/very/long/url/with/many/parameters";
        String longUrl2 = "https://www.another-example.com/some/other/very/long/path";

        // Shortening URLs
        String shortUrl1 = linkShortener.shortenUrl(longUrl1);
        String shortUrl2 = linkShortener.shortenUrl(longUrl2);
        System.out.println("Short URL for " + longUrl1 + " is: " + shortUrl1);
        System.out.println("Short URL for " + longUrl2 + " is: " + shortUrl2);

        // Expanding URLs
        System.out.println("Expanded URL for " + shortUrl1 + " is: " + linkShortener.expandUrl(shortUrl1));
        System.out.println("Expanded URL for " + shortUrl2 + " is: " + linkShortener.expandUrl(shortUrl2));

        // Testing for a non-existent short URL
        System.out.println("Expanded URL for non-existent short URL 'abc123' is: " + linkShortener.expandUrl("abc123"));
    }
}
