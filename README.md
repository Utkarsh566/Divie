# Divie
Utkarsh
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Valentines Day</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    .gradient-background {
      background: rgb(255, 208, 229);
      background: linear-gradient(180deg, rgba(255, 208, 229, 1) 0%, rgba(255, 232, 242, 1) 36%, rgba(255, 255, 255, 1) 100%);
    }

    .bounce2 {
      animation: bounce2 2s ease infinite;
    }

    @keyframes bounce2 {
      0%, 20%, 50%, 80%, 100% {
        transform: translateY(0);
      }
      40% {
        transform: translateY(-20px);
      }
      60% {
        transform: translateY(-10px);
      }
    }
  </style>
</head>
<body class="gradient-background">
  <div class="flex items-center justify-center h-screen">
    <div class="flex flex-col items-center p-4">
      <img id="imageDisplay" src="https://imgs.search.brave.com/8x-T7tRAk6tOBu5OKz3brC_VqMTZI65-ypjqe2iGggA/rs:fit:500:0:0:0/g:ce/aHR0cHM6Ly9tZWRp/YS5nZXR0eWltYWdl/cy5jb20vaWQvMjAw/MjI0MTA2LTAwMS9w/aG90by9raXR0ZW4t/aW4tZmxvd2Vycy5q/cGc_cz02MTJ4NjEy/Jnc9MCZrPTIwJmM9/VDFtYWNXZkVibWZl/NVpFZUFsQVk5SExu/T0pFaC1vNTVPbDFq/NXNMU3pUND0" alt="Cute kitten with flowers" class="rounded-md h-[300px]" style="object-fit: cover;" />
      <h2 id="valentineQuestion" class="text-4xl font-bold italic text-[#bd1e59] my-4">Will you be my Girlfriend?</h2>
      <div class="flex gap-4 pt-[20px] items-center" id="responseButtons">
        <button id="yesButton"
          class="bounce2 inline-flex items-center justify-center whitespace-nowrap rounded-md text-[20px] font-medium disabled:pointer-events-none disabled:opacity-50 hover:bg-green-400 min-h-12 min-w-[75px] px-4 py-2 bg-green-500 text-white transition">
          Yes
        </button>
        <button id="noButton"
          class="inline-flex items-center justify-center whitespace-nowrap rounded-md text-[20px] font-medium transition disabled:pointer-events-none disabled:opacity-50 hover:bg-red-700 h-12 min-w-[75px] w-auto px-4 py-2 bg-red-500 text-white">
          No
        </button>
      </div>
    </div>
  </div>

  <script type="module">
    import confetti from 'https://cdn.skypack.dev/canvas-confetti';
    const yesButton = document.getElementById('yesButton');
    const noButton = document.getElementById('noButton');
    const imageDisplay = document.getElementById('imageDisplay');
    const valentineQuestion = document.getElementById('valentineQuestion');
    const responseButtons = document.getElementById('responseButtons');
  
    let noClickCount = 0;
    let buttonHeight = 48; // Starting height in pixels
    let buttonWidth = 80;
    let fontSize = 20; // Starting font size in pixels
    const imagePaths = [
      "https://imgs.search.brave.com/KvdAcUCqW5sEwCgMq3wcJDNEedzp3xO88z6PjDX_Z2c/rs:fit:500:0:0:0/g:ce/aHR0cHM6Ly9tZWRp/YS5pc3RvY2twaG90/by5jb20vaWQvOTE2/MTU5NDE4L3Bob3Rv/L2N1dGUta2l0dGVu/LXBvcnRyYWl0LWJy/aXRpc2gtc2hvcnRo/YWlyLWNhdC5qcGc_/cz02MTJ4NjEyJnc9/MCZrPTIwJmM9Rnhl/cTBzeVdMeFFfaVpn/eGUyclN5LTFsLXRR/eHREVkdrRS0wTjAy/Z0Y5OD0",
      "https://imgs.search.brave.com/XLqD9GD9oKvXhTq0ytLGyCGqPQcGxf8EmavNHJ3gG44/rs:fit:500:0:0:0/g:ce/aHR0cHM6Ly93d3cu/Y2F0c3Rlci5jb20v/d3AtY29udGVudC91/cGxvYWRzLzIwMjMv/MTEvc2NhcmVkLWtp/dHRlbi1oaWRpbmct/S2hhbWlkdWxpbi1T/ZXJnZXktU2h1dHRl/cnN0b2NrLmpwZw",
      "https://imgs.search.brave.com/qzI5QNVA0d4JOKpKP_tbnOLR_2kGdWQu6duRUARinFM/rs:fit:500:0:0:0/g:ce/aHR0cHM6Ly9pLmt5/bS1jZG4uY29tL3Bo/b3Rvcy9pbWFnZXMv/bmV3c2ZlZWQvMDAx/LzM4NC81MzEvOGVk/LmpwZw",
      "https://imgs.search.brave.com/9Z41Q3G5SeMI-VcRmQtfR0_E6KuobocZOworVmgz0UQ/rs:fit:500:0:0:0/g:ce/aHR0cHM6Ly9tZWRp/YS5pc3RvY2twaG90/by5jb20vaWQvMTI4/MTcwMDg2My9waG90/by9raXR0ZW4tbG9v/a2luZy11cC5qcGc_/cz02MTJ4NjEyJnc9/MCZrPTIwJmM9S3VN/Mk9xNEFBTGljaU5L/ajdtRXRUQlFfU09C/WW0tck8yY1Awczdf/X0ZfYz0",
      "https://imgs.search.brave.com/JtdvO3cC6Ml8EY0u7f116FVUuvFXyYNdZR2ZUzBHes4/rs:fit:500:0:0:0/g:ce/aHR0cHM6Ly9tZWRp/YS5pc3RvY2twaG90/by5jb20vaWQvMTU3/NTkzMDkzL3Bob3Rv/L2tpdHRlbi1tYWtl/LWEtc3BlZWNoLmpw/Zz9zPTYxMng2MTIm/dz0wJms9MjAmYz1r/WE1QdVJtQk5FY0xi/QWVBLUhHdnNXdEs2/QnNFYlVOUnBRTGZE/dTN3YUZrPQ",
      "https://imgs.search.brave.com/s2ny6YwOWCCwqcHyYCMJRkYQuKIdiRn1HJ36sJlsaYs/rs:fit:500:0:0:0/g:ce/aHR0cHM6Ly9pbWcu/ZnJlZXBpay5jb20v/cHJlbWl1bS1waG90/by9wb3J0cmFpdC1r/aXR0ZW4tYmFza2V0/XzEwNDg5NDQtNzI4/NzI1NS5qcGc_c2Vt/dD1haXNfaHlicmlk",
      "https://imgs.search.brave.com/nalLXjVMn7Wyt6gqNZLsHQoHRI1BXtxW0VeKb2nrPc0/rs:fit:500:0:0:0/g:ce/aHR0cHM6Ly90My5m/dGNkbi5uZXQvanBn/LzAyLzA4LzgyLzc4/LzM2MF9GXzIwODgy/Nzg1MV9XbDdRWjM0/VlJkemZSN24wMEdV/S1NhUUhVSzE5VEF6/cS5qcGc"
    ];
  
    noButton.addEventListener('click', function() {
      if (noClickCount < 5) {
        noClickCount++;
        imageDisplay.src = imagePaths[noClickCount];
        buttonHeight += 35; // Increase height by 5px on each click
        buttonWidth += 35;
        fontSize += 25; // Increase font size by 1px on each click
        yesButton.style.height = `${buttonHeight}px`; // Update button height
        yesButton.style.width = `${buttonWidth}px`;
        yesButton.style.fontSize = `${fontSize}px`; // Update font size
        if (noClickCount < 6) {
          noButton.textContent = ["No", "Are you sure?", "Pookie please", "Don't do this to me :(", "You're breaking my heart", "I'm gonna cry..."][noClickCount];
        }
      }
    });
  
    yesButton.addEventListener('click', () => {
      imageDisplay.src = 'https://imgs.search.brave.com/iY_tq6OE_Sggctzigyhdw28iceUdQjkWNEM8LgjJ4I0/rs:fit:500:0:0:0/g:ce/aHR0cHM6Ly9tZWRp/YS5pc3RvY2twaG90/by5jb20vaWQvMTI2/NzAyMTA5Mi9waG90/by9mdW5ueS13aW5r/aW5nLWtpdHRlbi5q/cGc_cz02MTJ4NjEy/Jnc9MCZrPTIwJmM9/OVBvRllrcUtaMzBG/X3VieFg5MF9hendz/UjIyRU53ckZuT2p4/VjBSYW9Ubz0'; // Change to image7.gif
      valentineQuestion.textContent = "Yayyy!! Your gift is pending pookie..<3"; 
      
      // Change the question text
      responseButtons.style.display = 'none'; // Hide both buttons
      confetti(); // Trigger confetti animation
      
    });
    
  </script>  
  <H1> There is something for you ..!!!</H1>
  <h1>       <a href="https://youtu.be/2Vv-BfVoq4g?si=h2OHdLVmrRnGmO3i">CLICK HERE!!</a></h1>
</body>
</html>
