# Assignment
##Step 1: Set Up MySQL Database
 database rag_chatbot.
CREATE DATABASE rag_chatbot;
USE rag_chatbot;

CREATE TABLE documents (
    id INT AUTO_INCREMENT PRIMARY KEY,
    content TEXT,
    embedding BLOB
);
#Step 2: Backend in Java
##properties
spring.datasource.url=jdbc:mysql://localhost:3306/rag_chatbot
spring.datasource.username=root
spring.datasource.password=yourpassword
spring.jpa.hibernate.ddl-auto=update

#3.Document entity and a repository to interact with the database
@Entity
public class Document {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Lob
    private String content;

    @Lob
    private byte[] embedding;

    // getters and setters
}

public interface DocumentRepository extends JpaRepository<Document, Long> {
}
#4.Service for Embedding

@Service
public class EmbeddingService {
    public byte[] generateEmbedding(String text) {
      is a placeholder, you'll need to add the actual implementation
        return new byte[0];
    }
}

#5Controller:

##@RestController
@RequestMapping("/api")
public class DocumentController {
    @Autowired
    private DocumentRepository documentRepository;
    @Autowired
    private EmbeddingService embeddingService;

    @PostMapping("/upload")
    public ResponseEntity<?> uploadDocument(@RequestParam("content") String content) {
        byte[] embedding = embeddingService.generateEmbedding(content);
        Document document = new Document();
        document.setContent(content);
        document.setEmbedding(embedding);
        documentRepository.save(document);
        return ResponseEntity.ok("Document uploaded successfully");
    }

    @PostMapping("/chat")
    public ResponseEntity<?> chat(@RequestParam("query") String query) {
       
        return ResponseEntity.ok("Response from the RAG model");
    }
}

#Frontend with HTML/CSS/JS

##<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Upload Document</title>
</head>
<body>
    <h1>Upload Document</h1>
    <form id="uploadForm">
        <textarea id="content" placeholder="Enter text here"></textarea>
        <button type="submit">Upload</button>
    </form>
    <script>
        document.getElementById('uploadForm').addEventListener('submit', async (e) => {
            e.preventDefault();
            const content = document.getElementById('content').value;
            const response = await fetch('/api/upload', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/x-www-form-urlencoded'
                },
                body: new URLSearchParams({ content })
            });
            alert(await response.text());
        });
    </script>
</body>
</html>
#Chat Interface:
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Chat</title>
</head>
<body>
    <h1>Chat with RAG Model</h1>
    <div id="chatBox">
        <div id="messages"></div>
        <input type="text" id="query" placeholder="Type your message...">
        <button id="sendButton">Send</button>
    </div>
    <script>
        document.getElementById('sendButton').addEventListener('click', async () => {
            const query = document.getElementById('query').value;
            const response = await fetch('/api/chat', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/x-www-form-urlencoded'
                },
                body: new URLSearchParams({ query })
            });
            const result = await response.text();
            const messages = document.getElementById('messages');
            messages.innerHTML += `<p><b>You:</b> ${query}</p>`;
            messages.innerHTML += `<p><b>Bot:</b> ${result}</p>`;
        });
    </script>
</body>
</html>













