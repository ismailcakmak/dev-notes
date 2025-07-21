
# Start when you begin development
sudo systemctl start mongod

# Stop when done
sudo systemctl stop mongod

# Check MongoDB status
sudo systemctl status mongod

# View MongoDB logs
sudo journalctl -u mongod

# Connect to MongoDB shell
mongosh

# Stop MongoDB
sudo systemctl stop mongod

# Restart MongoDB
sudo systemctl restart mongod
