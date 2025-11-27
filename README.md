📘 AttendanceForm — Simple Solidity Attendance Smart Contract

A beginner-friendly Ethereum smart contract that allows users to mark their attendance on-chain with a single button click.
No complicated logic, no constructor inputs — just a clean and easy-to-understand Solidity project that is perfect for learning Web3 basics.

📝 Project Description

AttendanceForm is a simple smart contract built using Solidity that records student or participant attendance on the blockchain.
Each user can mark attendance once, and the contract stores a list of all attendees.

This project is ideal for:

Beginners learning Solidity

Students practicing Web3 basics

Hackathon mini-projects

Understanding mappings, arrays, and basic state changes in Ethereum

✅ What It Does

Allows any Ethereum address to mark their attendance

Stores attendance in a blockchain-verified list

Prevents duplicate attendance using a mapping

Anyone can read:

Total number of attendees

Full list of attendee addresses

Whether a given address has already attended

No wallet data, passwords, or personal information is required — attendance is recorded by wallet address only.

⭐ Features
🔹 Beginner-Friendly Solidity Code

Minimal and clean contract — easy to understand if you're new to smart contract development.

🔹 No Deployment Inputs

Just deploy and use immediately. No constructor params.

🔹 One-Click Attendance

Users simply call markAttendance() to register.

🔹 Public Attendance List

The contract makes it easy to fetch:

totalAttendees()

getAllAttendees()

🔹 Prevents Double Attendance

Smart contract ensures each wallet can attend only once.

🔹 Fully Transparent

Data is stored on-chain and visible to everyone.

🔗 Deployed Smart Contract

Contract Address: https://testnet.routescan.io/address/0x25bF5E5293782163e6580394A8a8e2C83a957536/contract/114/code

(Replace XXX with your deployed contract address when ready.)

📄 Smart Contract Code
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

/// @title Simple Attendance Form
/// @author
/// @notice Beginner contract to record attendance. No constructor inputs at deployment.
/// @dev Deployer becomes owner. Suitable for small-class demos (returning full arrays on-chain is OK for small lists).
contract AttendanceForm {
    address public owner;

    // attendees list (keeps order)
    address[] private attendees;

    // quick lookup: has this address marked attendance?
    mapping(address => bool) private hasAttendedMap;

    // record timestamp when address marked attendance
    mapping(address => uint256) private attendanceTimestamp;

    // events
    event AttendanceMarked(address indexed attendee, uint256 timestamp);
    event AttendanceCleared(address indexed by);

    // --- modifiers ---
    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner");
        _;
    }

    /// @notice Set the deployer as the owner. No deployment inputs required.
    constructor() {
        owner = msg.sender;
    }

    /// @notice Mark attendance for the sender. Can be called once per address.
    /// @dev Will revert if called again by same address.
    function markAttendance() external {
        require(!hasAttendedMap[msg.sender], "Already marked attendance");

        hasAttendedMap[msg.sender] = true;
        attendanceTimestamp[msg.sender] = block.timestamp;
        attendees.push(msg.sender);

        emit AttendanceMarked(msg.sender, block.timestamp);
    }

    /// @notice Check if an address has already marked attendance.
    /// @param user Address to check.
    /// @return true if user marked attendance, false otherwise.
    function hasAttended(address user) external view returns (bool) {
        return hasAttendedMap[user];
    }

    /// @notice Get timestamp when a user marked attendance (0 if not attended).
    /// @param user Address to query.
    /// @return unix timestamp of attendance or 0.
    function getAttendanceTimestamp(address user) external view returns (uint256) {
        return attendanceTimestamp[user];
    }

    /// @notice Number of attendees recorded.
    /// @return count of attendees.
    function getAttendeeCount() external view returns (uint256) {
        return attendees.length;
    }

    /// @notice Get attendee address by index (0-based).
    /// @param index Index into attendees array.
    /// @return address of attendee at given index.
    function getAttendee(uint256 index) external view returns (address) {
        require(index < attendees.length, "Index out of bounds");
        return attendees[index];
    }

    /// @notice Get all attendees (returns array). Good for small lists.
    /// @dev Avoid for very large lists (gas / UI concerns).
    function getAllAttendees() external view returns (address[] memory) {
        return attendees;
    }

    /// @notice Owner-only: clear all attendance records (resets state).
    /// @dev Emits AttendanceCleared. Use for starting a new session.
    function clearAttendance() external onlyOwner {
        // reset mapping entries for attendees to free space
        for (uint256 i = 0; i < attendees.length; i++) {
            address a = attendees[i];
            hasAttendedMap[a] = false;
            attendanceTimestamp[a] = 0;
        }
        // clear the array
        delete attendees;

        emit AttendanceCleared(msg.sender);
    }

    /// @notice Owner-only: change ownership if needed.
    /// @param newOwner new owner address.
    function transferOwnership(address newOwner) external onlyOwner {
        require(newOwner != address(0), "Zero address");
        owner = newOwner;
    }
}


(Paste the full Solidity contract here.)

🚀 How to Use
1. Clone or copy the contract into Remix

Visit: https://remix.ethereum.org

2. Compile

Use Solidity version 0.8.x.

3. Deploy

Select:

Environment: JavaScript VM / MetaMask / Hardhat network

Click Deploy

4. Interact

After deployment:

Click markAttendance()

Check totalAttendees()

View the full list using getAllAttendees()

📚 Concepts You Learn

State Variables

Mappings

Dynamic Arrays

Public Functions & Views

Basic Transaction Execution

Writing Gas-Efficient Beginner Logic

🧑‍💻 Ideal For

Web3 students

First-time Solidity learners

Blockchain semester projects

Hackathon submissions

Simple DApp backend prototype
