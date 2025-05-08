# Fusion App: Data & Privacy Information

This document outlines Fusion's data handling practices. It explains what user data Fusion handles, how it's used and shared, the security of stored credentials, and the implications of Fusion's addon system.

## Data Collection

According to the in-app privacy information:

*   The Fusion application **does not directly collect or transmit any personal information** from the user.
*   The only data handled by Fusion is information required to interact with **third-party services** that the user explicitly connects. Examples of such services mentioned are:
    *   Trakt
    *   TMDB
    *   MDBList
*   When users provide API keys or authenticate with these third-party services, this authentication information (e.g., API keys, tokens) is **stored locally on the user's device**.

## Data Usage

*   The locally stored authentication data is used solely to **enable the functionality of the connected third-party services** within Fusion.

## Data Sharing

*   The privacy information explicitly states that the locally stored API keys and authentication data **are not shared with Fusion's developers or any other external parties**.
*   Data is implicitly "shared" with the respective third-party services (Trakt, TMDB, MDBList) when Fusion interacts with their APIs using the user-provided credentials. This is a fundamental part of how these integrations work.
*   **iCloud Syncing:** If enabled by the user, certain app settings and data (like Trakt catalog lists, addon configurations, and other preferences) are synced via Apple's iCloud (`NSUbiquitousKeyValueStore`). This data is stored in the user's private iCloud account and is subject to Apple's iCloud security and privacy policies. Fusion does not directly access this iCloud data; it is managed by Apple's infrastructure.

## iCloud Data Syncing

Fusion offers an optional feature to sync certain data and settings using Apple's iCloud service (`NSUbiquitousKeyValueStore`).

### Data Synced with iCloud:

Data synced via iCloud may include:
*   General application settings and preferences.
*   Trakt service data, such as catalog lists and custom list names.
*   Configuration of user-installed addons, like the order of sections.
*   Other user-specific configurations designed to be consistent across devices.

This data is stored in the user's private iCloud account.

### Implications:

*   **Convenience:** iCloud syncing allows users to maintain consistent settings and data across multiple devices where they use Fusion and are signed into the same iCloud account.
*   **Data Accessibility:** Data synced to iCloud is stored on Apple's servers and is accessible to the user on any device connected to their iCloud account.
*   **Security:** The security of this data relies on the user's iCloud account security (e.g., strong password, two-factor authentication) and Apple's iCloud infrastructure security.
*   Fusion does not directly access data stored in a user's private iCloud `NSUbiquitousKeyValueStore`.

## Network Connections

Fusion makes the following types of network connections:

*   **Primary Connection to Trakt.tv API:**
    *   Fusion communicates extensively with the Trakt.tv API (`https://api.trakt.tv`).
    *   This is used for core features such as fetching user watch history, lists, playback progress, and for updating this information on Trakt.
    *   Authentication with Trakt is handled via OAuth.
*   **Secure HTTPS Connections:**
    *   Connections to the Trakt.tv API are made over **HTTPS**, ensuring that data transmitted between Fusion and Trakt's servers is encrypted.
*   **Metadata Image Loading:**
    *   Fusion fetches images (e.g., movie/show posters, backgrounds) from URLs. These URLs are likely provided by metadata services that Trakt integrates with (such as TMDB).
    *   These connections are also expected to be over HTTPS if the source URLs are secure.
*   **Connections to Other Third-Party Services (User-Initiated):**
    *   As stated in Fusion's privacy information, Fusion can also connect to other services like **TMDB** and **MDBList** if the user explicitly configures them.
    *   The nature of these connections would be to their respective APIs, presumably also over HTTPS.
*   **Addon-Specific Connections:**
    *   User-installed addons may make their own network connections to various content sources. The specifics of these connections would depend on the individual addons installed by the user.

### Addon Functionality and Data Handling:

Fusion supports an addon system that significantly extends its functionality. Addons are defined by a manifest file (typically JSON) usually fetched from a remote URL provided by the user.

*   **Manifest Contents:** An addon manifest (`FusionAddon` structure) defines:
    *   `id`, `name`, `version`, `description`.
    *   `endpoint`: A crucial URL that serves as the base for the addon's data operations (fetching catalogs, metadata).
    *   `resources`: Declares capabilities, such as providing "catalog" listings or "meta" (metadata).
    *   `types`: Supported media types (e.g., "movie", "series").
    *   `catalogs`: Specific content catalogs offered, including parameters they support (e.g., search, genre filters).

*   **Data Fetching Mechanism:**
    *   When Fusion needs data from an addon (e.g., populating a catalog or fetching metadata for an item), it constructs a request to the addon's specified `endpoint`.
    *   This request typically includes the resource type, item identifiers (like IMDB or TMDB IDs), and any user-specified or default parameters (e.g., search queries, genre selections).

*   **Implications for Data Privacy and Security:**
    *   **Arbitrary Remote Endpoints:** Each addon dictates its own server (`endpoint`) from which it sources data. Fusion will make network requests to these user-configured, potentially third-party, endpoints.
    *   **Security of Addon Endpoints:** The security (HTTPS vs. HTTP) and privacy practices of these addon endpoints are determined by the addon provider, not by Fusion.
    *   **Data Sent to Addons:** User search queries, selected filters, and media identifiers are sent by Fusion to the addon's `endpoint` as part of the data fetching process.
    *   **Data Received from Addons:** Fusion receives and processes structured data (media lists, metadata) from these endpoints.
    *   **No Direct Access to Sensitive App Data:** Addons define where Fusion should fetch data from; they do not have direct access to Fusion's internal sensitive data storage like the Keychain (e.g., Trakt tokens).
    *   **Trust Model:** Installing an addon implies a level of trust in the addon provider regarding the data sources they use and the security of their `endpoint`.

## Local Data Security and Credentials

Fusion employs the following measures for storing sensitive data locally on the user's device:

### API Key and Token Storage:

*   **Trakt.tv Access and Refresh Tokens:** To protect these sensitive credentials, Fusion stores Trakt tokens in the device's **Keychain**.
*   **TMDB API Key:** This key is stored by Fusion in **UserDefaults**.
*   **OpenRouter API Key:** This key is also stored by Fusion in **UserDefaults**.
*   **MDBList API Key:** This key is stored by Fusion in **UserDefaults** (under the key `"mdblistApiKey"`).
*   **General App Settings:** Other non-sensitive app settings and preferences are stored by Fusion in UserDefaults.

### Security Considerations:

*   Keychain storage provides robust protection for Trakt tokens.
*   API keys for TMDB, OpenRouter, and MDBList are stored in UserDefaults. Access to Fusion's data container on a compromised device could expose these keys.
*   Users can revoke API access from the respective third-party services if they suspect a compromise of their keys or accounts.
*   For iCloud synced data, strong iCloud account security (unique password, two-factor authentication) is crucial for protecting that information, which is managed by Apple's infrastructure.

## Further Information

This document aims to provide transparency into Fusion's data practices. For any questions or to view the latest in-app privacy information, please refer to the settings within the Fusion application. 