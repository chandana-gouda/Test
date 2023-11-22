1. Implement a circular queue,write functions: read_queue(), write_queue(), clear_queue(),When the queue is full, write queue should overwrite oldest data.

   PROGRAME
   
#include <iostream>

const int MAX_QUEUE_SIZE = 5;

class CircularQueue {
private:
    int* arr;
    int front;
    int rear;
    int size;
    int capacity;

public:
    CircularQueue(int capacity) : front(-1), rear(-1), size(0), capacity(capacity) {
        arr = new int[capacity];
    }

    ~CircularQueue() {
        delete[] arr;
    }

    bool isEmpty() {
        return size == 0;
    }

    bool isFull() {
        return size == capacity;
    }

    void writeQueue(int data) {
        if (isFull()) {
            std::cout << "Queue is full. Overwriting oldest data." << std::endl;
            front = (front + 1) % capacity; // Move front to overwrite the oldest data
        }

        rear = (rear + 1) % capacity;
        arr[rear] = data;
        size++;

        std::cout << "Write queue: " << data << std::endl;
    }

    void readQueue() {
        if (!isEmpty()) {
            std::cout << "Read queue: " << arr[front] << std::endl;
            front = (front + 1) % capacity;
            size--;
        } else {
            std::cout << "Queue is empty." << std::endl;
        }
    }

    void clearQueue() {
        while (!isEmpty()) {
            readQueue(); // Display elements before removing them
        }

        std::cout << "Queue cleared." << std::endl;
    }
};

int main() {
    CircularQueue myQueue(MAX_QUEUE_SIZE);

    std::cout << "Queue is empty." << std::endl;

    myQueue.writeQueue(1);
    myQueue.writeQueue(2);
    myQueue.writeQueue(3);
    myQueue.writeQueue(4);
    myQueue.writeQueue(5);

    myQueue.readQueue();

    myQueue.writeQueue(6);
    myQueue.readQueue();

    myQueue.clearQueue();

    return 0;
}                     
                         OUTPUT
            
Queue is empty.
Write queue: 1
Write queue: 2
Write queue: 3
Write queue: 4
Write queue: 5
Read queue: 1
Queue is full. Overwriting oldest data.
Write queue: 6
Read queue: 2
Read queue: 3
Read queue: 4
Read queue: 5
Read queue: 6
Queue cleared.


2. Write a function to extract the payload from the given data and return the payload data in a new array to the calling function

Given below is the data format of the input

command
Length
Data type
Data
2 bytes
2 bytes
1 byte
Length - 1 bytes
Includes Data_type paratmeter

Ex:

input_array[] = {00, 02, 00, 11, 01, 01, 02, 03, 04, 05, 06, 07, 08, 09, 10};

then output data ?​


             PROGRAME
  
#include <iostream>
#include <vector>
#include <iomanip>  // Add this line for setw and setfill

std::vector<uint8_t> extractPayload(const std::vector<uint8_t>& inputArray) {
    // Check if the input array has the minimum required length
    if (inputArray.size() < 5) {
        std::cout << "Invalid inputArray length" << std::endl;
        return {};
    }

    // Extract Length field
    size_t length = (inputArray[1] << 8) + inputArray[0];

    // Check if the inputArray has sufficient length for the specified payload
    if (inputArray.size() < length + 4) {
        std::cout << "Invalid inputArray length for the specified payload" << std::endl;
        return {};
    }

    // Extract the payload data
    std::vector<uint8_t> payloadData(inputArray.begin() + 4, inputArray.begin() + 4 + length);

    return payloadData;
}

int main() {
    // Corrected example usage:
    std::vector<uint8_t> inputArray = {0x02, 0x00, 0x00, 0x11, 0x01, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x10};
    std::vector<uint8_t> outputData = extractPayload(inputArray);

    // Check if the output data is not empty before printing
    if (!outputData.empty()) {
        // Print the output data with space separators and leading zeros
        for (const auto& value : outputData) {
            std::cout << std::setw(2) << std::setfill('0') << std::hex << static_cast<int>(value) << " ";
        }
        std::cout << std::endl;
    }

    return 0;
}
                       OUTPUT
          
           Output Data: 01 01 02 03 04 05 06 07 08 09 10 



